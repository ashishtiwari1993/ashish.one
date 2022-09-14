---
title: "Getting started with Elasticsearch"
date: 2022-09-14T17:44:43+05:30
draft: false
type: "post"
ogtype: "article"
tags: ["elasticsearch","kibana","analyzer","CRUD","BULK"]
slug: "ws-es"
categories: ["Elastic"]

---

# Sample Queries for Elasticsearch Workshop

## CRUD 

```

# insert

POST meetup/_doc/
{
  "name":"Ashish Tiwari"
}

# insert with id

POST meetup/_doc/1
{
  "name":"Ashish Tiwari"
}

# read

GET meetup/_search

# update

POST meetup/_doc/1
{
  "name":"Ashish",
  "company":"elastic",
  "address":"Navi Mumbai kharghar",
  "skills":{
    "language":["php","java","node"],
    "database":["mysql","mongodb"],
    "search":"elasticsearch"
  }
}

# search with query

GET meetup/_search
{
  "query": {
    "match": {
      "address": "navi"
    }
  }
}

# delete

DELETE meetup
```

## BULK

```
POST _bulk
{"index":{"_index":"meetup"}}
{"user_id":1,"first_name":"Yvonne","last_name":"Willmott","email":"ywillmott0@live.com","gender":"Female","street_address":"38 Helena Avenue","ip_address":"104.221.25.110","company":"Flashset"}
{"index":{"_index":"meetup"}}
{"user_id":2,"first_name":"Immanuel","last_name":"Philbrick","email":"iphilbrick1@wunderground.com","gender":"Male","street_address":"01 Bunting Pass","ip_address":"9.20.164.27","company":"Babblestorm"}
{"index":{"_index":"meetup"}}
{"user_id":3,"first_name":"Clotilda","last_name":"Danelut","email":"cdanelut2@deliciousdays.com","gender":"Agender","street_address":"0 Crowley Trail","ip_address":"158.94.144.140","company":"Riffpedia"}
{"index":{"_index":"meetup"}}
{"user_id":4,"first_name":"Nahum","last_name":"Attfield","email":"nattfield3@blog.com","gender":"Male","street_address":"7 Garrison Court","ip_address":"225.144.148.44","company":"Chatterpoint"}
{"index":{"_index":"meetup"}}
{"user_id":5,"first_name":"Vaughan","last_name":"Middis","email":"vmiddis4@ted.com","gender":"Male","street_address":"7 Cody Way","ip_address":"66.198.31.108","company":"Mynte"}
{"index":{"_index":"meetup"}}
{"user_id":6,"first_name":"Nolie","last_name":"Alessandrucci","email":"nalessandrucci5@networksolutions.com","gender":"Male","street_address":"826 Brown Hill","ip_address":"96.77.221.95","company":"Feedfish"}
{"index":{"_index":"meetup"}}
{"user_id":7,"first_name":"Beverlie","last_name":"Ovitts","email":"bovitts6@tripod.com","gender":"Male","street_address":"6 Sycamore Pass","ip_address":"102.24.117.107","company":"Zazio"}
{"index":{"_index":"meetup"}}
{"user_id":8,"first_name":"Graeme","last_name":"Dopson","email":"gdopson7@free.fr","gender":"Female","street_address":"26 Dunning Avenue","ip_address":"198.33.215.93","company":"Flashset"}
{"index":{"_index":"meetup"}}
{"user_id":9,"first_name":"Mellisa","last_name":"Hurich","email":"mhurich8@nbcnews.com","gender":"Male","street_address":"6371 Browning Way","ip_address":"66.0.3.199","company":"Divanoodle"}
{"index":{"_index":"meetup"}}
{"user_id":10,"first_name":"Dyan","last_name":"Loude","email":"dloude9@berkeley.edu","gender":"Female","street_address":"9818 Reindahl Road","ip_address":"16.56.137.54","company":"Agivu"}
{"index":{"_index":"meetup"}}
{"user_id":11,"first_name":"Becky","last_name":"Shank","email":"bshanka@tinypic.com","gender":"Male","street_address":"1206 Warrior Terrace","ip_address":"90.63.35.111","company":"Izio"}
{"index":{"_index":"meetup"}}
{"user_id":12,"first_name":"Bar","last_name":"Bedburrow","email":"bbedburrowb@vistaprint.com","gender":"Male","street_address":"75 Onsgard Crossing","ip_address":"85.122.33.250","company":"Zoombox"}
{"index":{"_index":"meetup"}}
{"user_id":13,"first_name":"Dorey","last_name":"Isenor","email":"disenorc@privacy.gov.au","gender":"Female","street_address":"53682 Parkside Crossing","ip_address":"150.158.150.213","company":"Rhyzio"}
{"index":{"_index":"meetup"}}
{"user_id":14,"first_name":"Torrin","last_name":"Rangall","email":"trangalld@buzzfeed.com","gender":"Male","street_address":"24247 Old Shore Plaza","ip_address":"40.151.17.2","company":"Devpoint"}
{"index":{"_index":"meetup"}}
{"user_id":15,"first_name":"Genvieve","last_name":"Beslier","email":"gbesliere@yolasite.com","gender":"Male","street_address":"96214 Miller Trail","ip_address":"115.143.68.208","company":"Oyonder"}
{"index":{"_index":"meetup"}}
{"user_id":16,"first_name":"Arden","last_name":"Ramas","email":"aramasf@whitehouse.gov","gender":"Polygender","street_address":"390 Gulseth Alley","ip_address":"36.83.126.154","company":"Youbridge"}
{"index":{"_index":"meetup"}}
{"user_id":17,"first_name":"Alyosha","last_name":"Domm","email":"adommg@washingtonpost.com","gender":"Female","street_address":"32 Oxford Way","ip_address":"174.71.176.45","company":"Wikizz"}
```

## Upload sample json data from kibana

Download [movies.json](/data/movies.json) and insert into elasticsearch by using below command:

*Open Kibana -> Menu -> Home -> Upload a file*

## Create Data view in Kibana

*Open Kibana -> Menu -> Stack Management -> Data Views (Kibana)*

## Create Dashboard

*Open Kibana -> Menu -> Analytics -> Dashboard*

## _analyze

```
# analyze

GET /_analyze?pretty
{
  "text" : "Quick Brown Foxes!"
}

# hindi analyzer

GET /_analyze?pretty
{
  "analyzer": "hindi", 
  "text" : "चाणक्य ने चंद्रगुप्त और बिंदूसार दोनों के लिए प्रधानमंत्री और राजनयिक सलाहकार के रूप में काम किया।"
}

# marathi analyzer
```
