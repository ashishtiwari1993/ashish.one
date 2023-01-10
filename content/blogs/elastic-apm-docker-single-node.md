---
title: "Start a single node elastic cluster with Docker Compose"
date: 2022-06-08T13:25:09+05:30
draft: false
type: "post"
ogtype: "article"
tags: ["elasticsearch","kibana","elastic-apm","apm-server","docker","elastic-stacks","elastic-stacks-docker"]
slug: "elastic-docker-compose"
categories: ["Elastic"]

---

# Introduction

In this gist, we will quickly try to spin Elastic stacks with Docker containers. We are going to use [docker-compose](https://docs.docker.com/compose/). You can learn more about [Docker](https://www.docker.com/) & [Docker Compose](https://docs.docker.com/compose/), Which will help you to understand the flow.  

# Prerequisite

Tested on the below configuration.

* docker:`Docker version 20.10.16, build aa7e414`
* docker-compose:`Docker version 20.10.16, build aa7e414`

# Cluster

This cluster will include

* Elasticsearch
* Kibana
* Logstash
* APM

# `.env` file

## Make directory

```sh
mkdir docker-elastic
cd docker-elastic
```

## Create `.env` file

```sh
 # Password for the 'elastic' user (at least 6 characters)
ELASTIC_PASSWORD=pass@123

# Password for the 'kibana_system' user (at least 6 characters)
KIBANA_PASSWORD=pass@123

# Version of Elastic products
STACK_VERSION=8.6.0

# Set the cluster name
CLUSTER_NAME=docker-cluster

# Set to 'basic' or 'trial' to automatically start the 30-day trial
LICENSE=basic
#LICENSE=trial

# Port to expose Elasticsearch HTTP API to the host
ES_PORT=9200
#ES_PORT=127.0.0.1:9200

# Port to expose Kibana to the host
KIBANA_PORT=5601
#KIBANA_PORT=80

# Port to expose APM Server to the host
APM_PORT=8200

# Port to expose Logstash Server to the host

# Here we are not specifying any port for logstash.
# But if you want to use any port in the input plugin,
# Feel free to mention in the docker-compose.yml file.
LOGSTASH_PIPELINE_PATH=/tmp/pipeline/

# Increase or decrease based on the available host memory (in bytes)
MEM_LIMIT=1073741824

# Project namespace (defaults to the current folder name if not set)
#COMPOSE_PROJECT_NAME=myproject
```

Save and close.

# Create `docker-compose.yml` file 

```yaml
version: "2.2"

services:
  setup:
    image: docker.elastic.co/elasticsearch/elasticsearch:${STACK_VERSION}
    volumes:
      - certs:/usr/share/elasticsearch/config/certs
    user: "0"
    command: >
      bash -c '
        if [ x${ELASTIC_PASSWORD} == x ]; then
          echo "Set the ELASTIC_PASSWORD environment variable in the .env file";
          exit 1;
        elif [ x${KIBANA_PASSWORD} == x ]; then
          echo "Set the KIBANA_PASSWORD environment variable in the .env file";
          exit 1;
        fi;
        if [ ! -f config/certs/ca.zip ]; then
          echo "Creating CA";
          bin/elasticsearch-certutil ca --silent --pem -out config/certs/ca.zip;
          unzip config/certs/ca.zip -d config/certs;
        fi;
        if [ ! -f config/certs/certs.zip ]; then
          echo "Creating certs";
          echo -ne \
          "instances:\n"\
          "  - name: es01\n"\
          "    dns:\n"\
          "      - es01\n"\
          "      - localhost\n"\
          "    ip:\n"\
          "      - 127.0.0.1\n"\
          > config/certs/instances.yml;
          bin/elasticsearch-certutil cert --silent --pem -out config/certs/certs.zip --in config/certs/instances.yml --ca-cert config/certs/ca/ca.crt --ca-key config/certs/ca/ca.key;
          unzip config/certs/certs.zip -d config/certs;
        fi;
        echo "Setting file permissions"
        chown -R root:root config/certs;
        find . -type d -exec chmod 750 \{\} \;;
        find . -type f -exec chmod 640 \{\} \;;
        echo "Waiting for Elasticsearch availability";
        until curl -s --cacert config/certs/ca/ca.crt https://es01:9200 | grep -q "missing authentication credentials"; do sleep 30; done;
        echo "Setting kibana_system password";
        until curl -s -X POST --cacert config/certs/ca/ca.crt -u elastic:${ELASTIC_PASSWORD} -H "Content-Type: application/json" https://es01:9200/_security/user/kibana_system/_password -d "{\"password\":\"${KIBANA_PASSWORD}\"}" | grep -q "^{}"; do sleep 10; done;
        echo "All done!";
      '      
    healthcheck:
      test: ["CMD-SHELL", "[ -f config/certs/es01/es01.crt ]"]
      interval: 1s
      timeout: 5s
      retries: 120

  es01:
    depends_on:
      setup:
        condition: service_healthy
    image: docker.elastic.co/elasticsearch/elasticsearch:${STACK_VERSION}
    volumes:
      - certs:/usr/share/elasticsearch/config/certs
      - esdata01:/usr/share/elasticsearch/data
    ports:
      - ${ES_PORT}:9200
    environment:
      - node.name=es01
      - cluster.name=${CLUSTER_NAME}
      - cluster.initial_master_nodes=es01
      - discovery.seed_hosts=""
      - ELASTIC_PASSWORD=${ELASTIC_PASSWORD}
      - bootstrap.memory_lock=true
      - xpack.security.enabled=true
      - xpack.security.http.ssl.enabled=true
      - xpack.security.http.ssl.key=certs/es01/es01.key
      - xpack.security.http.ssl.certificate=certs/es01/es01.crt
      - xpack.security.http.ssl.certificate_authorities=certs/ca/ca.crt
      - xpack.security.http.ssl.verification_mode=certificate
      - xpack.security.transport.ssl.enabled=true
      - xpack.security.transport.ssl.key=certs/es01/es01.key
      - xpack.security.transport.ssl.certificate=certs/es01/es01.crt
      - xpack.security.transport.ssl.certificate_authorities=certs/ca/ca.crt
      - xpack.security.transport.ssl.verification_mode=certificate
      - xpack.license.self_generated.type=${LICENSE}
    mem_limit: ${MEM_LIMIT}
    ulimits:
      memlock:
        soft: -1
        hard: -1
    healthcheck:
      test:
        [
          "CMD-SHELL",
          "curl -s --cacert config/certs/ca/ca.crt https://localhost:9200 | grep -q 'missing authentication credentials'",
        ]
      interval: 10s
      timeout: 10s
      retries: 120

  apm:
    depends_on:
      es01:
        condition: service_healthy
    image: docker.elastic.co/apm/apm-server:${STACK_VERSION}
    volumes:
      - certs:/usr/share/apm-server/certs
    ports:
      - ${APM_PORT}:8200
    command: >
      apm-server -e
         -E output.elasticsearch.hosts=["es01:9200"]
         -E output.elasticsearch.protocol=https
         -E output.elasticsearch.username=elastic
         -E output.elasticsearch.password=${ELASTIC_PASSWORD}
         -E output.elasticsearch.ssl.enabled=true
         -E output.elasticsearch.ssl.certificate_authorities=/usr/share/apm-server/certs/ca/ca.crt
         -E output.elasticsearch.ssl.certificate=/usr/share/apm-server/certs/es01/es01.crt
         -E output.elasticsearch.ssl.key=/usr/share/apm-server/certs/es01/es01.key      

  kibana:
    depends_on:
      es01:
        condition: service_healthy
    image: docker.elastic.co/kibana/kibana:${STACK_VERSION}
    volumes:
      - certs:/usr/share/kibana/config/certs
      - kibanadata:/usr/share/kibana/data
    ports:
      - ${KIBANA_PORT}:5601
    environment:
      - SERVERNAME=kibana
      - ELASTICSEARCH_HOSTS=https://es01:9200
      - ELASTICSEARCH_USERNAME=kibana_system
      - ELASTICSEARCH_PASSWORD=${KIBANA_PASSWORD}
      - ELASTICSEARCH_SSL_CERTIFICATEAUTHORITIES=config/certs/ca/ca.crt
    mem_limit: ${MEM_LIMIT}
    healthcheck:
      test:
        [
          "CMD-SHELL",
          "curl -s -I http://localhost:5601 | grep -q 'HTTP/1.1 302 Found'",
        ]
      interval: 10s
      timeout: 10s
      retries: 120

  logstash:
    depends_on:
      es01:
        condition: service_healthy
    image: docker.elastic.co/logstash/logstash:${STACK_VERSION}
    volumes:
      - certs:/usr/share/logstash-server/certs
      - ${LOGSTASH_PIPELINE_PATH}:/usr/share/logstash/pipeline/
    environment:
      - SERVERNAME=kibana
      - ELASTICSEARCH_HOSTS=https://es01:9200
      - ELASTICSEARCH_USERNAME=elastic
      - ELASTICSEARCH_PASSWORD=${ELASTIC_PASSWORD}
      - ELASTICSEARCH_SSL_CERTIFICATEAUTHORITIES=/usr/share/logstash-server/certs/ca/ca.crt
    mem_limit: ${MEM_LIMIT}
     
volumes:
  certs:
    driver: local
  esdata01:
    driver: local
  kibanadata:
    driver: local
```

Save and close.

# Start the cluster

## Start

```sh
docker-compose up -d
```

## Stop

```sh
docker-compose down
```

## Stop with deleting network, containers and volumes

```sh
docker-compose down -v
```

# Instruction

## Logstash

1. You need to have pipeline configuration files on `LOGSTASH_PIPELINE_PATH` location. If there will be no file, Logstash will throw an error and get exit.

# NOTE 

> You can simply comment other stacks which is not needed. For example if you want to just run Elasticsearch & Kibana, Just comment the APM or other stack specification.

