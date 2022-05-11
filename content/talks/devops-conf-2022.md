---
title: "Parse custom logs in Elasticsearch using grok_pattern"
date: 2022-03-06T14:30:32+05:30
draft: false
type: "post"
ogtype: "article"
tags: ["elasticsearch","grok_pattern","log_parse","observability"]
categories: ["Elastic"]
cover:
    image: "/img/misc/devops-conf-2022.jpeg"
    alt: "devops conf 2022 cover pic"
    caption: "Parse custom logs in elasticsearch"
    relative: false
    hidden: true
---

# Introduction

In the talk, Quickly showed how you can use the [metricbeat](https://www.elastic.co/beats/metricbeat) to check system health. With a few clicks, you can start monitoring your infra. 

Another demo shows, How you can parse your custom unstructured logs to Elasticsearch. There is some utility already available for predefined logs format like json, apache, nginx and system logs etc.

But if you have different requirement where you need to parse a different types of log format , You can parse using [Grok processor](https://www.elastic.co/guide/en/elasticsearch/reference/master/grok-processor.html).

Also explained how you can read custom logs from log files in real time and ingest to Elasticsearch. 


# Slides

{{< gslides src="https://docs.google.com/presentation/d/e/2PACX-1vQtGa1x8-WMwWX8MTMZ-8GdJDMmiro4dNwcIE5HHmj1oGaSs88znwGf-W8bwUSANkIRQkeMZnh8KfWs/embed?start=false&loop=false&delayms=3000" >}}

## Talk Video

{{< youtube id="owyv9sQE7q8?start=4856&end=9427" >}}


#### Feel free to comment below, If you have any doubts or suggestion about this talk. 
