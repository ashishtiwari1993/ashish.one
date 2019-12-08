---
title: "How to scale with massive update queries in Elasticsearch?"
date: 2019-12-08T20:26:06+05:30
draft: false
type: "talk"
tags: ["elasticsearch","update","scale","mysql"]
slug: "scale-with-massive-updates-queries-in-elasticsearch"
---
# Introduction

What this talk is all about?
We recently moved from MySQL to Elasticsearch where we got a direct 10x - 15x boost in our performance.  

We came up with unique use cases of heavy updates in Elasticsearch. That been challenging but yes currently Our Elaticsearch handling 200 million requests per day very efficiently. Our WRITE consist of the partial update, update with script conditions and of course simple indexing.  

Our READ request is 1 million/day which contains Scroll, simple search & Aggregations Query. We achieved to display our email logs in next to real-time.
We also worked on Disk optimization by believing in the principle of "Know your query". Currently having 6TB + of the cluster with 80 GB ingestion per day.  
  
<iframe src="//www.slideshare.net/slideshow/embed_code/key/fSQDmWeE9XypiR" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/pepipost/scale-with-massiveupdates" title="How to scale with massive update queries in Elastic Search?" target="_blank">How to scale with massive update queries in Elastic Search?</a> </strong> from <strong><a href="//www.slideshare.net/pepipost" target="_blank">Pepipost</a></strong> </div>


<br>
<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/NSZXMv0va74?start=000&end=2574" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
