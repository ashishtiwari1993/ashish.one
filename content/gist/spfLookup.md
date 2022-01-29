---
title: "SPF Lookup in Go"
date: 2020-08-03T00:38:53+05:30
draft: false
ogtype: "gist"
tags: ["SPF","spf","spflookup","go","golang"]
slug: "spf-lookup-in-go"
topics: "Golang"
---

In this gist, We will check how we can extract SPF records in Go.


## Prerequisite

### Go version

```sh
$ go version
go version go1.13 linux/amd64
```

### Dependency

[DNS Library(https://github.com/miekg/dns)](https://github.com/miekg/dns)

## Install dependency

```sh
$ go get github.com/miekg/dns 
```

## spfLookup.go

{{< gist ashishtiwari1993 ea5da5581ae2c56f3e25f8509155bd37 >}}

Here you can change `nameserver` according to your requirement. I have specified here google's name server (`8.8.8.8`). You can also use cloudflare's nameserver [(1.1.1.1)](1.1.1.1)

## Conclusion

You can make any DNS query with `miekg/dns` library. In the above script, we have looked up `TXT` Records and then we have searched for a string containing `v=spf1`.
