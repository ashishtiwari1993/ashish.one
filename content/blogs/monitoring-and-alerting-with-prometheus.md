---
title: "Monitoring and Alerting With Prometheus - 1"
date: 2019-09-22T15:20:39+05:30
type: "post"
ogtype: "article"
tags: ["prometheus","alertmanager"]
draft: false
---

As a developer, many times you would have worried, whether your services are up and running or not. Not only that, sometimes as an infrastructure guy you might be also worried about your server’s health too. What is the current RAM or disk utilization? or whether they are going to be fully occupied which in turn can completely bring the system down. These are just the basics and in fact there are tons of more such things which need to be monitored and fixed in everyday's life.

*“Never send a human to do a machine's job”*

At some time, deploying humans to check all these and that too for 24 * 7 can be a real pain.

That's exactly the purpose of writing this tutorial series around monitoring and alerting. In this tutorial, you will learn how to setup Prometheus as a universal monitoring system and how to use its exporter to define and fetch the metrics which are really important to track.

## What is Prometheus?

Prometheus is an open-source system for monitoring and alerting. It developed in the GO language. It is currently a standalone open source project and maintained independently by any organization. You can check more details [here](https://prometheus.io/docs/introduction/overview/#what-is-prometheus). 

## Why Prometheus?

1. Open-source and of course freely available :)
2. It is constantly contributed by the community. It is stable and used by many good brands.  Check [Stackshare](https://stackshare.io/prometheus).
3. Good community support and well documented. 
4. You do not need any big infrastructure to get started. It can be started with 1 GB RAM.
5. It has its own UI to check any metrics. But, many prefer Grafana with Prometheus which gives you better visualization on your Prometheus metrics. 
6. Lots of pre-build Grafana dashboard and exporters already written. You have to just reuse those exporters.
You can check the list of Prometheus features [here](https://prometheus.io/docs/introduction/overview/#features).

[continue reading](https://pepipost.com/tutorials/setup-prometheus-and-exporters/)

<script src="https://utteranc.es/client.js"
        repo="ashishtiwari1993/ashish.one"
        issue-term="title"
        label="Comment"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>
