---
title: "How to scale with massive update queries in Elasticsearch?"
date: 2019-12-08T20:26:06+05:30
draft: false
type: "post"
tags: ["elastic","elasticsearch","update","scale","mysql"]
slug: "scale-with-massive-updates-queries-in-elasticsearch"
categories: ["Elastic"]
---
# Introduction

## What this talk is all about?
We recently moved from MySQL to Elasticsearch where we got a direct 10x - 15x boost in our performance.  

We came up with unique use cases of heavy updates in Elasticsearch. That been challenging but yes currently Our Elaticsearch handling 200 million requests per day very efficiently. Our WRITE consist of the partial update, update with script conditions and of course simple indexing.  

Our READ request is 1 million/day which contains Scroll, simple search & Aggregations Query. We achieved to display our email logs in next to real-time.
We also worked on Disk optimization by believing in the principle of "Know your query". Currently having 6TB + of the cluster with 80 GB ingestion per day.  
 
## Slides 


{{< gslides src="https://docs.google.com/presentation/d/e/2PACX-1vRiOzGgkrN1dO7fD4MDUKzr8WIhHqIhS5Iw1N27_mxYVdtPYcK17ib6ZTdg3bgExVuccJ35vxUSNP3X/embed?start=false&loop=false&delayms=3000" >}}


## Talk Video

{{< youtube NSZXMv0va74 >}}

<iframe width="560" height="315" src="https://www.youtube.com/embed/NSZXMv0va74?start=000&end=2574" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

####  Feel free to comment below, If you have any doubts or suggestion about this talk.

