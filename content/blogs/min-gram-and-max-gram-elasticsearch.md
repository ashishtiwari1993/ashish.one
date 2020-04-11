---
title: "What should be the value of max_gram and min_gram in Elasticsearch?"
date: 2019-09-22T15:20:39+05:30
type: "post"
ogtype: "article"
tags: ["min_gram","max_gram","tokenizer","elasticsearach"]
draft: false
---

I was working on elasticsearch and the requirement was to implement like query “%text%” ( like mysql %like% ). We could use wildcard, regex or query string but those are slow. Hence i took decision to use [ngram token filter](https://www.elastic.co/guide/en/elasticsearch/reference/6.2/analysis-ngram-tokenfilter.html) for like query. It was quickly implemented on local and works exactly i want.

## The problem

To know the actual behavior, I implemented the same on staging server. I found some problem while we start indexing on staging.

1. Storage size was directly increase by 8x, Which was too risky. In my previous index the string type was “keyword”. Its took approx 43 gb to store the same data. I implemented a new schema for “like query” with ngram filter which took below storage to store same data.

```
curl -XGET http://localhost:9200/_cat/indices?v

index       docs.count  pri.store.size
ngram-test  459483245   329.5gb

```

2. Sometime like query was not behaving properly. Not getting exact output.

## Schema

```
curl -XPUT "localhost:9200/ngram-test?pretty" -H 'Content-Type: application/json' -d'
{
  "settings":{
    "index":{
      "number_of_shards":5,
      "number_of_replicas":0,
      "codec": "best_compression"
    },
    "analysis":{
      "filter":{
        "like_filter":{
          "type":"ngram",
          "min_gram":3,
          "max_gram":10,
          "token_chars":[
            "letter",
            "digit",
            "symbol"
          ]
        }
      },
      "analyzer":{
        "like_analyzer":{
          "type":"custom",
          "tokenizer":"keyword",
          "filter":[
            "lowercase",
            "like_filter"
          ]
        }
      }
    }
  },
  "mappings":{
    "logs":{
      "properties":{
        "email":{
          "type":"keyword",
          "fields":{
            "text":{
              "analyzer":"like_analyzer",
              "search_analyzer":"like_analyzer",
              "type":"text"
            }
          }
        }
      }
    }
  }
}'
```

## Analyzing the behavior of ngram filter

We made one test index and start monitoring by inserting doc one by one.

[Continue Reading](https://medium.com/@ashishstiwari/what-should-be-the-value-of-max-gram-and-min-gram-in-elasticsearch-f091404c9a14)


