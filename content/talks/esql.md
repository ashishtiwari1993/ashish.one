---
title: "Elasticsearch Query Language (ES|QL)"
date: 2024-02-01T10:54:52+05:30
draft: false
type: "post"
ogtype: "article"
tags: ["elasticsearch","kibana","analyzer","CRUD","BULK","ESQL","query","langugate"]
slug: "esql"
categories: ["Elastic"]

---

## Introduction

[ES|QL](https://www.elastic.co/guide/en/elasticsearch/reference/current/esql.html) is a new query language for Elasticsearch. It is the unified language for all kinds of use cases like simple queries, aggregations, performing correlations, finding logs, etc. It provides simple easy syntax to perform complex queries. If you come from SQL background, You going to find this very handy. 

It is a piped separated langugage with a combination of [source commands](https://www.elastic.co/guide/en/elasticsearch/reference/current/esql-commands.html#esql-source-commands) and [process](https://www.elastic.co/guide/en/elasticsearch/reference/current/esql-commands.html#esql-processing-commands) commands. The Elasticsearch Query Language (ES|QL) makes use of "pipes" (|) to manipulate and transform data in a step-by-step fashion. This means output of the first step will go as an input for second step.

ES|QL is more than langugage. The execution engine is developed by considering performance in mind. Here ES|QL is not going to convert into Query DSL instead it will be directly executed within Elasticsearch. It operates on blocks at a time instead of per row, targets vectorization and cache locality, and embraces specialization and multi-threading.

> ES|QL - Filter, Transform and Analyze

## Example

Below is a few examples of ES|QL. I am considering you have an Elasticsearch and kibana is [installed](https://www.elastic.co/search-labs/tutorials/install-elasticsearch) and running. Please [import the sample dataset (Sample web logs)](https://www.elastic.co/guide/en/kibana/current/get-started.html#gs-get-data-into-kibana) from kibana. Navigate in side menu  -> `Management` -> `Dev Tools`  to perform the below query.

### Query

```
# Format

POST /_query?format=csv
{
  "query": """
    from kibana_sample_data_logs
  """
}

# keep

POST _query?format=txt
{
  "query": """
    from kibana_sample_data_logs
    | keep @timestamp, clientip, host, tags, bytes
  """
}

# where, limit

POST _query?format=txt
{
  "query": """
    from kibana_sample_data_logs
    | keep @timestamp, clientip, host, tags, bytes
    | where bytes > 1000
    | limit 5
  """
}

```

### Data enrichment (ENRICH)

```

# Create mappings

PUT lang
{
  "mappings": {
    "properties": {
      "lang_id": {
        "type": "keyword"
      },
      "name": {
        "type": "keyword"
      }
    }
  }
}

PUT devs
{
  "mappings": {
    "properties": {
      "lang_id": {
        "type": "keyword"
      },
      "name": {
        "type": "keyword"
      }
    }
  }
}

# Create index "lang"

PUT lang/_bulk
{ "index" : {}}
{ "lang_id": "1x", "name": "java" }
{ "index" : {}}
{ "lang_id": "2x", "name": "php" }
{ "index" : {}}
{ "lang_id": "3x", "name": "node" }
{ "index" : {}}
{ "lang_id": "4x", "name": "python" }
{ "index" : {}}
{ "lang_id": "5x", "name": "ruby" }

# create index "devs"

PUT devs/_bulk
{ "index" : {}}
{ "lang_id": "5x", "developer": "bob" }
{ "index" : {}}
{ "lang_id": "3x", "developer": "mark" }
{ "index" : {}}
{ "lang_id": "1x", "developer": "max" }
{ "index" : {}}
{ "lang_id": "2x", "developer": "david" }
{ "index" : {}}
{ "lang_id": "4x", "developer": "ashish" }


# Create enrich policy

PUT /_enrich/policy/dev_lang
{
  "match": {
    "indices": "lang",
    "match_field": "lang_id",
    "enrich_fields": ["name"]
  }
}

PUT /_enrich/policy/dev_lang/_execute?wait_for_completion=true

POST _query?format=txt
{
  "query":"""
    from devs
    | keep lang_id, name, developer
    | enrich dev_lang on lang_id with name
  """
}

```


### GROK

```
POST _query?format=txt
{
  "query":"""
    from kibana_sample_data_logs
    | grok agent "%{WORD:browser}/%{NUMBER:version}"
    | keep browser, version, @timestamp
  """
}

POST _query?format=txt
{
  "query":"""
    from kibana_sample_data_logs
    | grok agent "%{WORD:browser}/%{NUMBER:version}"
    | keep browser, version, @timestamp
    | stats count(*) by version
  """
}
```

### DISSECT

```
POST _query?format=txt
{
  "query": """
    from kibana_sample_data_logs 
    | dissect message "%{ip} - - %{time} %{web_call} "
    | keep ip, time, web_call
  """
}
```

