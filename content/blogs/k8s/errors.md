---
title: "Kubernetes generic errors"
date: 2023-01-07T22:41:02+05:30
draft: false
type: "post"
ogtype: "article"
slug: "k8s-generic-errors"
tags: ["k8s","kubernetes","errors","k8s-errors","generic-errors"]
slug: "k8s-generic-errors"
categories: ["Kubernetes"]

---

## 1. unknown service runtime.v1alpha2.ImageService

> Error: pulling image: rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.ImageService

### System configuration

```
centos 9 / 2GB RAM / 2CPU
```


### Master Node

Same issue on master node.

#### Command

```sh
[root@kube-master-1 ~]# kubeadm config images pull
failed to pull image "registry.k8s.io/kube-apiserver:v1.26.0": output: E0107 14:52:09.997544    4134 remote_image.go:222] "PullImage from image service failed" err="rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.ImageService" image="registry.k8s.io/kube-apiserver:v1.26.0"
time="2023-01-07T14:52:09Z" level=fatal msg="pulling image: rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.ImageService"
, error: exit status 1
To see the stack trace of this error execute with --v=5 or higher
```

#### ✅ Solved

Remove below file:

```sh
rm /etc/containerd/config.toml
```

Try again.



### Worker Node

Same issue on worker node while joining to master.

#### Command

```sh
[root@kube-worker-2 ~]# kubeadm join x.x.x.x:6443 --token ga8bqg.01azxe9avjx2n6jr        --discovery-token-ca-cert-hash sha256:d57699d74721094e5f921d48a0f9f895a0d7def7e1977e95ce0027a03e7f7d39
[preflight] Running pre-flight checks
error execution phase preflight: [preflight] Some fatal errors occurred:
	[ERROR CRI]: container runtime is not running: output: E0107 17:46:12.269694   11160 remote_runtime.go:948] "Status from runtime service failed" err="rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.RuntimeService"
time="2023-01-07T17:46:12Z" level=fatal msg="getting status of runtime: rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.RuntimeService"
, error: exit status 1
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
To see the stack trace of this error execute with --v=5 or higher
```

#### ✅ Solved

Same remove the `config.toml` file and restart the `containerd` service.

```sh
rm /etc/containerd/config.toml
systemctl restart containerd
```
