---
title: "Set Up TLS for Elasticsearch on Public IP / Domain"
date: 2024-10-23T20:29:51+05:30
draft: false
type: "post"
ogtype: "article"
tags: ["elasticsearch","tls","security","ssl"]
slug: "setup-tls-elasticsearch"
categories: ["Elastic"]
---

# Setup TLS on public IP or Domain with Elasticsearch

This is a quick gist to demonstrate how you can run Elasticsearch securely with TLS on a public IP or domain. Sometimes, we need to spin up Elasticsearch for a development environment. In that case, you can follow these quick steps.

> Note - This gist is not recommended for production multinode cluster. As I only tested on single node cluster. If you have a multi-node cluster please follow the official [guide](https://www.elastic.co/guide/en/elasticsearch/reference/current/manually-configure-security.html).

# Elasticsearch Installation

1. I created a VM instance on GCP. Here are the details -  

OS: `Ubuntu 22.04`  
Machine type: `e2-standard-4`  
Configuration: `4 CPU, 16 GB RAM`  

2. [Configure static external IP](https://cloud.google.com/compute/docs/ip-addresses/configure-static-external-ip-address) for newly created VM or ensure your VM is accessible from the public IP.

3. Installed Elasticsearch from [Archive](https://www.elastic.co/guide/en/elasticsearch/reference/current/targz.html).

You can [verify](https://www.elastic.co/guide/en/elasticsearch/reference/current/targz.html#_check_that_elasticsearch_is_running) if Elasticsearch is running or not. 

Get into the `$ES_HOME` folder -

```sh
cd elasticsearch-{version}/
```

In my case it was
```sh
cd elasticsearch-8.15.3/
```

# Default TLS setup

When you install Elasticsearch, By default it will generate a TLS certificate for the host `localhost` and IP `127.0.0.1`. 

```sh
ls config/certs/
```

```sh
http.p12  http_ca.crt  transport.p12
```

CURL call -

```sh
curl --cacert $ES_HOME/config/certs/http_ca.crt -u elastic:pass https://localhost:9200
```

OR

```sh
curl --cacert $ES_HOME/config/certs/http_ca.crt -u elastic:pass https://127.0.0.1:9200
```

Response

```json
{
  "name" : "benchmark1",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "kqZsQ5WrTAWnlFmYbyMQWw",
  "version" : {
    "number" : "8.15.3",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "f97532e680b555c3a05e73a74c28afb666923018",
    "build_date" : "2024-10-09T22:08:00.328917561Z",
    "build_snapshot" : false,
    "lucene_version" : "9.11.1",
    "minimum_wire_compatibility_version" : "7.17.0",
    "minimum_index_compatibility_version" : "7.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

If you run the same CURL command using the public IP of the VM, It won’t work.

```sh
curl --cacert $ES_HOME/config/certs/http_ca.crt -u elastic:pass https://x.x.x.x:9200
```

```
curl: (60) SSL certificate problem: unable to get local issuer certificate
More details here: https://curl.se/docs/sslcerts.html

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the web page mentioned above.
```

It is giving an error because the certificate is not valid for the IP / Domain. However, you can call insecurely by adding `-k` which is not recommended for production use.

```
curl -k --cacert $ES_HOME/config/certs/http_ca.crt -u elastic:pass https://x.x.x.x:9200
```

# Generate TLS certificate for the Public IP / Domain

## 1. Let's generate the new local certificate authority CA)

```sh
./bin/elasticsearch-certutil ca
```

```
This tool assists you in the generation of X.509 certificates and certificate
signing requests for use with SSL/TLS in the Elastic stack.

The 'ca' mode generates a new 'certificate authority'
This will create a new X.509 certificate and private key that can be used
to sign certificate when running in 'cert' mode.

Use the 'ca-dn' option if you wish to configure the 'distinguished name'
of the certificate authority

By default the 'ca' mode produces a single PKCS#12 output file which holds:
    * The CA certificate
    * The CA's private key

If you elect to generate PEM format certificates (the -pem option), then the output will
be a zip file containing individual files for the CA certificate and private key

Please enter the desired output file [elastic-stack-ca.p12]: 
Enter password for elastic-stack-ca.p12 :
```

I kept blank for the last two questions. But you can change the output file and set the Password.

It must have created a file `elastic-stack-ca.p12` in `$ES_HOME` directory.

## 2. Generate the Certificate using newly created CA

```sh
./bin/elasticsearch-certutil http
```

It will ask some questions -

1. Generate a CSR? [y/N] `n`
2. Use an existing CA? [y/N] `y`
3. CA Path: `$ES_HOME/elastic-stack-ca.p12` 
> You need to provide the absolute path for `elastic-stack-ca.p12`.

4. Password for elastic-stack-ca.p12:
> Type the password if you had set in Step 1. In my case, I'll keep it blank. 

5. For how long should your certificate be valid? [5y]

6. Generate a certificate per node? [y/N] `y`
7. node #1 name: `benchmakr1.node`
8. Which hostnames will be used to connect to benchmakr1.node?

```
yourdomain.name  
localhost
```

9. Which IP addresses will be used to connect to benchmakr1.node?

```
127.0.0.1  
x.x.x.x
```

> Add your public IP list.

10. Do you wish to change any of these options? [y/N] `n`
11. Generate additional certificates? [Y/n] `n`
12. Provide a password for the "http.p12" file:

> Type the password which you want to set and save it. I kept it as a blank. 

13. What filename should be used for the output zip file?

It must have created `elasticsearch-ssl-http.zip` file 

## 3. Set `http.p12` 

Lets unzip

```
mkdir new-ssl
mv elasticsearch-ssl-http.zip new-ssl/
cd new-ssl/
unzip elasticsearch-ssl-http.zip
```

This will create two directory -

```
1. elasticsearch
2. kibana
```

go to the `$ES_HOME` directory

```
cd ..
```

Replace old `http.p12` with new.

```sh
cp new-ssl/elasticsearch/http.p12 config/certs/http.p12
```

## 4. Change password for your private key `http.p12`

```
./bin/elasticsearch-keystore add xpack.security.http.ssl.keystore.secure_password

```

> Setting xpack.security.http.ssl.keystore.secure_password already exists. Overwrite? [y/N] `y`

```
Enter value for xpack.security.http.ssl.keystore.secure_password:
```

Kept it as a blank in my case. But if you have set any password on `Step 2 (Question no. 12)`, Please enter the same password.

## 5. Restart the Elasticsearch and verify

Let's make the same CURL call with the newly generated CA in `kibana` folder

```sh
curl --cacert $ES_HOME/new-ssl/kibana/elasticsearch-ca.pem -u elastic:pass https://x.x.x.x:9200
```

```sh
{
  "name" : "benchmark1",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "kqZsQ5WrTAWnlFmYbyMQWw",
  "version" : {
    "number" : "8.15.3",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "f97532e680b555c3a05e73a74c28afb666923018",
    "build_date" : "2024-10-09T22:08:00.328917561Z",
    "build_snapshot" : false,
    "lucene_version" : "9.11.1",
    "minimum_wire_compatibility_version" : "7.17.0",
    "minimum_index_compatibility_version" : "7.0.0"
  },
  "tagline" : "You Know, for Search"
}
```


> This gist for single node cluster. But if you're looking to secure your cluster end to end, Go through the complete [guide](https://www.elastic.co/guide/en/elasticsearch/reference/8.15/secure-cluster.html).  
