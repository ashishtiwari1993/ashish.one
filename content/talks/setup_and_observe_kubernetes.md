---
title: "Setup & Observe Kubernetes cluster"
date: 2024-03-28T21:41:03+05:30
draft: false
type: "post"
tags: ["elasticsearch","kubernetes","Elastic","observability","k8s"]
categories: ["Elastic"]
cover:
    #image: "/img/misc/k8s_monitoring.jpg" # image path/url
    alt: "Monitor kubernetes cluster with Elastic Observability" # alt text
    caption: "cover pic" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page

---

# Introduction

In this gist we will quickly spin a sample Kubernetes cluster and deploying the nginx pod. Additionally, we will implement monitoring using Elastic.

# Setup K8s cluster

## Cluster architecture

3 Node cluster

Machine - Centos7, 4GB RAM 

1. kube1.local  - Control plane node
2. kube2.local - worker node
3. kube3.local - worker node

Here I am setting hostname `kube1.local`, `kube2.local`, `kube3.local`.  Login into all of the servers and perform below command on all three nodes. 

## Swap off

```sh
sudo swapoff -a
```

## Install docker

You can refer [docker documentation](https://docs.docker.com/engine/install/centos/) but here are the quick steps:


## Contanerd runtime

kubeadm automatically tries to detect an installed container runtime by scanning through a list of known endpoints.

Verify if containerd is running or not

```sh
ps -ef | grep containerd
```

In my system containerd was running. [Install containerd](https://github.com/containerd/containerd/blob/main/docs/getting-started.md) if it is not installed. 

Change below cofing to remove `cri` from `disable_plugins`

```sh
sudo vim /etc/containerd/config.toml
#disabled_plugins = ["cri"]
disabled_plugins = []

sudo systemctl restart containerd
```

## Install kubeadm, kubelet & kubectl

```sh
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://pkgs.k8s.io/core:/stable:/v1.28/rpm/
enabled=1
gpgcheck=1
gpgkey=https://pkgs.k8s.io/core:/stable:/v1.28/rpm/repodata/repomd.xml.key
exclude=kubelet kubeadm kubectl cri-tools kubernetes-cni
EOF


# Set SELinux in permissive mode (effectively disabling it)
sudo setenforce 0

sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

sudo systemctl enable --now kubelet
```

Make sure you install same version on all of the nodes. In my case i am installing `v1.28`.

Need to install all three packages on all nodes

**Kubeadm** - Use to bootstrap the cluster

**Kubelet** - Will run all over the machin and take care of pods and container (General operation like stop / start / modify)

**Kubectl** - command line utility to talk with kubernetes APIs

## On control plane node only (kube1.local)

Initialise kubeadm

```sh
kubeadm init
```

On success, It will print the below `join` command, which we will use to join any machine in the cluster.

```sh
kubeadm join control_plane_ip:6443 --token tlif0t.jq7r1nvdmltre7m3 --discovery-token-ca-cert-hash sha256:02eb1a7f249b2f2a2b8db2b8fef9b5564ac3db9d42da39db71c23c06df5cecb8
```

Save this command. We need to execute this command on every worker node to join the cluster.

## On worker node only (kube2.local, kube3.local)

Perform join command.

```sh
kubeadm join control_plane_ip:6443 --token tlif0t.jq7r1nvdmltre7m3 --discovery-token-ca-cert-hash sha256:02eb1a7f249b2f2a2b8db2b8fef9b5564ac3db9d42da39db71c23c06df5cecb8
```

You should see the message - “This node has joined the cluster” with addition details.

## Verify cluster (kube1.local)

Enable `kubectl` to run from non root user.

```sh
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

List nodes

```sh
kubectl get nodes

NAME          STATUS   ROLES           AGE     VERSION
kube1.local   Ready    control-plane   199d    v1.28.1
kube2.local   Ready    <none>          8m22s   v1.28.8
kube3.local   Ready    <none>          199d    v1.28.1
```

Our cluster is up and running.

# Deploy Nginx (kube1.local)

Create Nginx deployment

```sh
cat <<EOF | kubectl apply -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
EOF
```

Expose the Nginx deployment on a NodePort 32000

```sh
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  type: NodePort
  ports:
    - port: 80
      targetPort: 80
      nodePort: 32000
EOF
```

Verify pod is running

```sh
Kubectl get pods
```

Just visit on browser - http://external_id:32000.

You should able to see the nginx’s default page.

# Setup Observability with Elastic

## Enable kube-state-matrics (kube1.local)

```sh
git clone https://github.com/kubernetes/kube-state-metrics
kubectl apply -f kube-state-metrics/examples/standard/
```

## Verify endpoint

```sh
kubectl port-forward svc/kube-state-metrics -n kube-system 8080:8080

#login to anoter tab and hit
curl localhost:8080/metrics
```

It should return all metrics.

## Monitoring using Elastic cloud

You can follow detailed doc - [https://www.elastic.co/getting-started/observability/monitor-kubernetes-clusters](https://www.elastic.co/getting-started/observability/monitor-kubernetes-clusters)

Blog - [https://www.elastic.co/blog/kubernetes-cluster-metrics-logs-monitoring](https://www.elastic.co/blog/kubernetes-cluster-metrics-logs-monitoring)



# Reference talk

Bring logs, metrics, and traces from your Kubernetes cluster and the workloads running on it into a single, unified solution. Elastic observability gives better visibility on your kubernetes ecosystem where you can monitor your pods, services, workload etc. Use a centrally managed Elastic Agent to gain visibility into your Kubernetes deployments on EKS, AKS, GKE or self-managed clusters.

## Talk Video

{{< youtube 8qOt_gYjwcw >}}
