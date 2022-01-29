---
title: "Golang basics & Handling 100k hourly webhooks with golang @MimePost"
date: 2020-03-11T00:12:30+05:30
draft: false
ogtype: "article"
tags: ["golang","talk","concurrency"]
slug: "golang-basics-and-send-100k-hourly-webhooks-with-golang-mimepost"
topics: "Golang"
type: "post"
---

## What this talk about?

I am working on Golang for the last 1 year from the published date. I have shared some basics of Golang. 

Also, shared What are the pain points developers face when they migrate from any other language (Especially from web language like PHP) to Golang? 

I have explained the Webhook architecture of MimePost And how we sending 100k Request hourly( Though Benchmark proves we can scale up to 500k).

Shown some immature code which given me a better understanding of 100% CPU utilization and How I waste my major time to debug on silly things. 

Shared one of our error and it's solutions related to How you can avoid race conditions on "map" type variables.

## Slides

{{< gslides src="https://docs.google.com/presentation/d/e/2PACX-1vQExSl-gRPoA9hC6qXuqrjwiQVHAanDieZN_5GpV2Lw9cuxjsVFEN_wkTThqpQwZ36vJz4zwmTvV7cC/embed?start=false&loop=false&delayms=3000" >}}


## Talk Video

{{< youtube ZjOcwoCkkog >}}


## Demo - 1 

This code will make 100% CPU utilization and `forever()` function not going to share any single CPU time with `anotherGoroutine()`

{{< gist ashishtiwari1993 00b2a56ac8f5b39d3229d723e58815bc >}}

## Demo - 2 

Reproduce Golang "fatal error: concurrent map writes" & Solution. To reproduce comment Mutex related all operation like line no. 12, 30, 32, 44, 46. Mutex is use to prevent race condition which generates this error.

{{< gist ashishtiwari1993 d494b71ac264184ba46ced1bf2114c30 >}}

Find more details on this [blog](https://ashish.one/blogs/fatal-error-concurrent-map-writes/).

#### Feel free to comment below, If you have any doubts or suggestion about this talk.

