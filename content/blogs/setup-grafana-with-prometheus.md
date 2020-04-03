---
title: "[Part 4] Setup Grafana With Prometheus"
date: 2020-04-03T21:32:48+05:30
draft: false
type: "post"
ogtype: "article"
tags: ["prometheus","grafana","grafana-dashboard","node-exporter","dashboard","alertmanager"]
slug: "setup-grafana-with-prometheus"
---

As you know Prometheus already having UI (`localhost:9090`). But it is not enough to give you better visualization on one screen. For better visualization and a graphical representation, we are going to use Grafana.

## What is Grafana?
As [grafana.com](https://grafana.com) says 

> **”Grafana is the open-source analytics and monitoring solution for every database.”**

This means Grafana is an independent tool for analytics and monitor which gives your various types of Graphs. It is not restricted to Prometheus DB only, You can use mostly any Databases like MySQL, Elasticsearch, etc. So you can visualize different data points from the different databases on one screen. This is the flexibility and power Grafana provides. 

Grafana works on TSDB(Time Series Database) or Your data should be save in time series manner. Check explaination [here](https://grafana.com/docs/grafana/latest/guides/timeseries/).

It has an alert system. You can configure an alert on Grafana itself for any Metric.

## Why Grafana?

1. Open-source of course freely Available
2. It is constantly contributed by the community. It is stable and used by many good brands.
3. Good community support and well documented.
4. You do not need any big infrastructure to get started.
5. Lots of pre-build Grafana dashboards already available and build by the community.  So it will be a rare case where you have to build your dashboard.

## My local system configuration:

* 8GB RAM
* Ubuntu 18.04 LTS 

You can check [here](https://grafana.com/docs/grafana/latest/installation/requirements/) your system requirement according to your Operating System. Also, They explained different ways to install Grafana for different OS. 

## Setup Grafana

### Installation

Grafana has two types of Software:

1. __Enterprise Release__
2. __Open-Sources Software(OSS) or Community Release__

We are going to install OSS Release.

#### Step 1: Install `apt-transport-https`

```sh
$ sudo apt-get install -y apt-transport-https
``` 

![apt-transport-https](/img/grafana-setup/apt-transport-https.png)

#### Step 2: Install `wget`

```sh
$ sudo apt-get install -y software-properties-common wget
```

![install-wget](/img/grafana-setup/install-wget.png)

#### Step 3: Add key

```sh
$ wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
```

#### Step 4: Add `apt` Repository

```sh
$ sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
```

```sh
OK
```

#### Step 5: Update & Install


```sh
$ sudo apt-get update
$ sudo apt-get install grafana
```

![install-grafana](/img/grafana-setup/install-grafana.png)

#### Step 6: Start `grafana-server`

```sh
$ sudo systemctl daemon-reload  
$ sudo systemctl start grafana-server  
$ sudo systemctl status grafana-server  
```

![grafana-status](/img/grafana-setup/grafana-status.png)

#### Step 7: Visit `localhost:3000`

![grafana-browser](/img/grafana-setup/grafana-browser.png)


### Install with Docker

Ignore this part if you not using docker.

```
docker run -d -p 3000:3000 grafana/grafana
```

visit `localhost:3000`.

We have successfully installed Grafana. Let's configure with Prometheus.

## Configure with Prometheus

We already setup Prometheus in [part 1](https://ashish.one/blogs/setup-prometheus-and-exporters/). Now I am considering your Prometheus and Node exporter are running on port `9090` & `9100` respectively.

### Setup Grafana <--> Prometheus 

#### Step 1: Login on Grafana

Visit `localhost:3000`. 3000 is the default port of Grafana, However, you can run on any port. To change the port see the [configuration](https://grafana.com/docs/grafana/latest/installation/configuration/). 

Login into Grafana with default Credentials:

__Username__:`admin`  
__Password__:`admin`

Now it will ask you to change your password. Once done, Click on `save`, You will be redirect on the dashboard.


![grafana-dashboard](/img/grafana-setup/grafana-dashboard.png)

#### Step 2: Add Data Source

Goto Sidebar & Navigate:

**_Configuration -> Data Sources_**

![datasource](/img/grafana-setup/datasource.png)


click on `Add data source`.
![prometheus-data-source](/img/grafana-setup/prometheus-data-source.png)

Search for `Prometheus` & click on `Select`.

Add Prometheus endpoint in `URL` and click on `save & Test`.


![grafana-prometheus-config](/img/grafana-setup/grafana-prometheus-config.png)

You will get the notification for success. Here we have successfully integrated Grafana and Prometheus.

#### Step 3: Add Dashboard

Here we have two option available:

1. **_Create your Dashboard_**
2. **_Import any pre-build Dashboard_**

We will go with the second option because that is the beauty of community :) People already build a dashboard for various stacks.

You can search for Dashboard from [https://grafana.com/grafana/dashboards](https://grafana.com/grafana/dashboards) as per your requirement.

Here is the link for Node exporter dashboard: [https://grafana.com/grafana/dashboards/1860](https://grafana.com/grafana/dashboards/1860) 
 
Here Dashboard ID is `1860`. Every dashboard which contributed in Grafana has unique ID. You can just import any dashboard by inserting the ID. 

![node-exporter-dashboard](/img/grafana-setup/node-exporter-dashboard.png)


On Grafana Goto sidebar & Navigate:

**_+(Plus/Add sign) -> Import_**


Enter Dashboard Id and click on `load`. After that   
![dashboard-id](/img/grafana-setup/dashboard-id.png)


Enter the Name & select Prometheus on label `Prometheus` as shown in below image.
![dashboard-config](/img/grafana-setup/dashboard-config.png)

Click on `Import`.

Your Dashboard will be ready.

![node-dashboard](/img/grafana-setup/node-dashboard.png)


Here we have successfully integrated the Grafana with Prometheus. You can explore more features of Grafana like Alerts, Users, etc. 

For the alerting system, I prefer Prometheus Alertmanager only. You can check more details on the Alertmanager in part 2. It gives you more control and advanced feature. 

But every tool has use cases and built for special purposes. You can choose according to your requirements. 


**In case of any confusion or issues leave comments below :)**

<script src="https://utteranc.es/client.js"
        repo="ashishtiwari1993/ashish.one"
        issue-term="title"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>

