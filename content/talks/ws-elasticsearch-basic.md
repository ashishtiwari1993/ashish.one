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

# Insert

POST meetup/_doc/
{
  "name":"Ashish Tiwari"
}

# Insert with id

POST meetup/_doc/1
{
  "name":"Ashish Tiwari"
}

# Search

GET meetup/_search

# Update

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

## Create mapping

```
PUT /devfest-raipur
{
  "mappings": {
    "properties": {
      "age":    { "type": "integer" },  
      "email":  { "type": "keyword"  }, 
      "name":   { "type": "text"  }     
    }
  }
}

```

## Analyze

```
# analyze

GET /_analyze?pretty
{
  "text" : "Quick Brown Foxes!"
}

# Whitespace

GET _analyze
{
  "analyzer": "whitespace",
  "text": "The 2 QUICK Brown-Foxes jumped over the lazy dog\u0027s bone."
}
```

### What is an analyzer?
An analyzer is made of character filters, tokenizer and token filters.

Let's build one

```
POST _analyze
{
  "char_filter": [], 
  "tokenizer":   "standard",
  "filter":      [], 
  "text": [ 
    "I like when the <strong>quick</strong> foxes jumps over lazy DOGS!",
    "and <strong>fast</strong>"
  ]
}

```

Let's remove the html code.

```
POST _analyze
{
  "char_filter": ["html_strip"], 
  "tokenizer":   "standard",
  "filter":      [], 
  "text": [ 
    "I like when the <strong>quick</strong> foxes jumps over lazy DOGS!",
    "and <strong>fast</strong>"
  ]
}
```

Some words don't bring us any value. Let's skip them.

```
POST _analyze
{
  "char_filter": ["html_strip"], 
  "tokenizer":   "standard",
  "filter":      [
    {
      "type":       "stop",
      "stopwords":  [ "_english_"]
    }
  ], 
  "text":
  [ 
    "I like when the <strong>quick</strong> foxes jumps over lazy DOGS!",
    "and <strong>fast</strong>"
  ]
}
```

We can also remove "I", "when" and "over".

```
POST _analyze
{
  "char_filter": ["html_strip"], 
  "tokenizer":   "standard",
  "filter":      [
    {
      "type":       "stop",
      "ignore_case":true, 
      "stopwords":  [ "_english_", "I", "when", "over"]
    }
  ], 
  "text":
  [ 
    "I like when the <strong>quick</strong> foxes jumps over lazy DOGS!",
    "and <strong>fast</strong>"
  ]
}
```


`DOGS` and `dogs` should match.

```
POST _analyze
{
  "char_filter": ["html_strip"], 
  "tokenizer":   "standard",
  "filter":      [
    {
      "type":       "stop",
      "ignore_case":true, 
      "stopwords":  [ "_english_", "I", "when", "over"]
    },
    "lowercase"
  ], 
  "text":
  [ 
    "I like when the <strong>quick</strong> foxes jumps over lazy DOGS!",
    "and <strong>fast</strong>"
  ]
}
```

`dog`, `dogs` and `fox`, `foxes` and `jump`, `jumps`, `jumping`, `jumped` should match. Let's use a `stemmer`.

```
POST _analyze
{
  "char_filter": ["html_strip"], 
  "tokenizer":   "standard",
  "filter":      [
    {
      "type":       "stop",
      "ignore_case":true, 
      "stopwords":  [ "_english_", "I", "when", "over"]
    },
    "lowercase",
    {
      "type":       "stemmer",
      "language":   "english" 
    }
  ], 
  "text":
  [ 
    "jumping jumps jump jumped",
    "I like when the <strong>quick</strong> foxes jumps over lazy DOGS!",
    "and <strong>fast</strong>"
  ]
}
```

# Language analyzer

```
GET /_analyze?pretty
{
  "analyzer": "hindi", 
  "text" : "चाणक्य ने चंद्रगुप्त और बिंदूसार दोनों के लिए प्रधानमंत्री और राजनयिक सलाहकार के रूप में काम किया।"
}
```


## Let's create Hindi search engine (गीतयंत्र)

### Create mapping

```
PUT geetyantra
{
  "settings": {
    "analysis": {
      "analyzer": {
        "geetyantra-analyzer":{
          "type":"hindi"
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "adhyay": {"type": "integer"},
      "updesh": {"type": "text","analyzer": "geetyantra-analyzer"}
    }
  }
}
```

### Insert data

Taking data from 🙏[विकिपीडिया-श्रीमद्भगवद्गीता](https://hi.wikipedia.org/wiki/%E0%A4%B6%E0%A5%8D%E0%A4%B0%E0%A5%80%E0%A4%AE%E0%A4%A6%E0%A5%8D%E0%A4%AD%E0%A4%97%E0%A4%B5%E0%A4%A6%E0%A5%8D%E0%A4%97%E0%A5%80%E0%A4%A4%E0%A4%BE)

Lets insert some data like below:

```
POST geetyantra/_doc
{
  "adhyay":1,
  "updesh":"कृष्ण ने अर्जुन की वह स्थिति देखकर जान लिया कि अर्जुन का शरीर ठीक है किंतु युद्ध आरंभ होने से पहले ही उस अद्भुत क्षत्रिय का मनोबल टूट चुका है। बिना मन के यह शरीर खड़ा नहीं रह स।"
}

POST geetyantra/_doc
{
  "adhyay":2,
  "updesh":"दूसरे अध्याय का नाम सांख्ययोग है। इसमें जीवन की दो प्राचीन संमानित परंपराओं का तर्कों द्वारा वर्णन आया है। अर्जुन को उस कृपण स्थिति में रोते देखकर कृष्ण ने उनका ध्यान दिलाया है कि इस प्रकार का क्लैव्य और हृदय की क्षुद्र दुर्बलता अर्जुन जैसे वीर के लिए उचित नहीं।"
}

POST geetyantra/_doc
{
  "adhyay":3,
  "updesh":"नित्य कर्म करने वाले की श्रेष्ठता"
}

POST geetyantra/_doc
{
  "adhyay":4,
  "updesh":"चौथे अध्याय में, जिसका नाम ज्ञान-कर्म-संन्यास-योग है, यह बाताया गया है कि ज्ञान प्राप्त करके कर्म करते हुए भी कर्मसंन्यास का फल किस उपाय से प्राप्त किया जा सकता है।",
  "shloka":"यदा यदा हि धर्मस्य ग्लानिर्भवति भारत,अभ्युत्थानमधर्मस्य तदात्मानं सृजाम्यहम्"
}

POST geetyantra/_doc
{
  "adhyay":5,
  "updesh":"ज्ञानी महापुरुष विद्या-विनययुक्त ब्राह्मण में और चाण्डाल में तथा गाय, हाथी एवं कुत्ते में भी समरूप परमात्मा को देखने वाले होते हैं।",
  "shloka":"विद्याविनयसंपन्ने ब्राह्मणे गवि हस्तिनि,शुनि चैव श्वपाके च पंडिता: समदर्शिन"
}


POST geetyantra/_doc
{
  "adhyay":6,
  "updesh":"छठा अध्याय आत्मसंयम योग है जिसका विषय नाम से ही प्रकट है। जितने विषय हैं उन सबसे इंद्रियों का संयम-यही कर्म और ज्ञान का निचोड़ है। सुख में और दुख में मन की समान स्थिति, इसे ही योग कहते हैं।"
}

POST geetyantra/_doc
{
  "adhyay":7,
  "updesh":"पंचतत्व, मन, बुद्धि भी मैं हूँ| मैं ही संसार की उत्पत्ति करता हूँ और विनाश भी मैं ही करता हूँ। मेरे भक्त चाहे जिस प्रकार भजें परन्तु अंततः मुझे ही प्राप्त होते हैं। मैं योगमाया से अप्रकट रहता हूँ और मुर्ख मुझे केवल साधारण मनुष्य ही समझते हैं।",
  "shloka":"यो यो यां यां तनुं भक्तः श्रद्धयार्चितुमिच्छति,तस्य तस्याचलां श्रद्धां तामेव विदधाम्यहम्"
}

POST geetyantra/_doc
{
  "adhyay":8,
  "updesh":"की संज्ञा अक्षर ब्रह्मयोग है। उपनिषदों में अक्षर विद्या का विस्तार हुआ। गीता में उस अक्षरविद्या का सार कह दिया गया है-अक्षर ब्रह्म परमं, अर्थात् परब्रह्म की संज्ञा अक्षर है। मनुष्य, अर्थात् जीव और शरीर की संयुक्त रचना का ही नाम अध्यात्म है।"
}

POST geetyantra/_doc
{
  "adhyay":9,
  "updesh":"को राजगुह्ययोग कहा गया है, अर्थात् यह अध्यात्म विद्या विद्याराज्ञी है और यह गुह्य ज्ञान सबमें श्रेष्ठ है। राजा शब्दका एक अर्थ मन भी था। अतएव मन की दिव्य शक्तिमयों को किस प्रकार ब्रह्ममय बनाया जाय, इसकी युक्ति ही राजविद्या है।"
}

POST geetyantra/_doc
{
  "adhyay":10,
  "updesh":"दसवें अध्याय का नाम विभूतियोग है। इसका सार यह है कि लोक में जितने देवता हैं, सब एक ही भगवान, की विभूतियाँ हैं, मनुष्य के समस्त गुण और अवगुण भगवान की शक्ति के ही रूप हैं।"
}

POST geetyantra/_doc
{
  "adhyay":11,
  "updesh":"का नाम विश्वरूपदर्शन योग है। इसमें अर्जुन ने भगवान का विश्वरूप देखा। विराट रूप का अर्थ है मानवीय धरातल और परिधि के ऊपर जो अनंत विश्व का प्राणवंत रचनाविधान है, उसका साक्षात दर्शन। विष्णु का जो चतुर्भुज रूप है, वह मानवीय धरातल पर सौम्यरूप है।"
}

POST geetyantra/_doc
{
  "adhyay":12,
  "updesh":"का नाम भक्ति योग है। जो जानने योग्य है। जिसको जानकर मनुष्य परमानन्द को प्राप्त हो जाता है अर्थात वो परमात्मा ही सत्य है ।।"
}

POST geetyantra/_doc
{
  "adhyay":13,
  "updesh":"में एक सीधा विषय क्षेत्र और क्षेत्रज्ञ का विचार है। यह शरीर क्षेत्र है, उसका जाननेवाला जीवात्मा क्षेत्रज्ञ है।"
}

POST geetyantra/_doc
{
  "adhyay":14,
  "updesh":"का नाम गुणत्रय विभाग योग है। यह विषय समस्त वैदिक, दार्शनिक और पौराणिक तत्वचिंतन का निचोड़ है-सत्व, रज, तम नामक तीन गुण-त्रिको की अनेक व्याख्याएँ हैं।"
}

POST geetyantra/_doc
{
  "adhyay":15,
  "updesh":"का नाम पुरुषोत्तमयोग है। इसमें विश्व का अश्वत्थ के रूप में वर्णन किया गया है। यह अश्वत्थ रूपी संसार महान विस्तारवाला है। देश और काल में इसका कोई अंत नहीं है।"
}

POST geetyantra/_doc
{
  "adhyay":16,
  "updesh":"में देवासुर संपत्ति का विभाग बताया गया है। आरंभ से ही ऋग्देव में सृष्टि की कल्पना दैवी और आसुरी शक्तियों के रूप में की गई है। "
}

POST geetyantra/_doc
{
  "adhyay":17,
  "updesh":"की संज्ञा श्रद्धात्रय विभाग योग है। इसका संबंध सत, रज और तम, इन तीन गुणों से ही है, अर्थात् जिसमें जिस गुण का प्रादुर्भाव होता है, उसकी श्रद्धा या जीवन की निष्ठा वैसी ही बन जाती है।"
}

POST geetyantra/_doc
{
  "adhyay":18,
  "updesh":"की संज्ञा मोक्षसंन्यास योग है। इसमें गीता के समस्त उपदेशों का सार एवं उपसंहार है। यहाँ पुन: बलपूर्वक मानव जीवन के लिए तीन गुणों का महत्व कहा गया है। पृथ्वी के मानवों में और स्वर्ग के देवताओं में कोई भी ऐसा नहीं जो प्रकृति के चलाए हुए इन तीन गुणों से बचा हो।"
}
```


### Search


```

GET geetyantra/_search?size=20

GET geetyantra/_search
{
  "query": {
    "match": {
      "updesh": "अर्जुन"
    }
  }
}

GET geetyantra/_search
{
  "query": {
    "match": {
      "shloka": "धर्मस्य"
    }
  }
}

GET geetyantra/_search
{
  "query": {
    "multi_match": {
      "query": "श्रद्धा",
      "fields": ["shloka","updesh"]
    }
  }
}

GET geetyantra/_search
{
  "query": {
    "match_phrase_prefix": {
      "updesh": "चौथे अध्याय में"
    }
  }
}
```

## Search as you type

Refer article [Search as you type](https://ashish.one/blogs/search-as-you-type/).
