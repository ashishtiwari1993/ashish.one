---
title: "Workshop - Leverage ChatGPT with Elasticsearch"
date: 2023-07-21T15:52:06+05:30
draft: false
type: "post"
tags: ["chatgpt","elasticsearch","LLM","knn","hybrid-search"]
ogtype: "article"
slug: "chatgpt-elasticsearch"
categories: ["elastic"]
showToc: true
TocOpen: true
draft: false
hidemeta: false
description: "Connect ChatGPT to proprietary data stores using Elasticsearch"
disableHLJS: false # to disable highlightjs
disableShare: false
hideSummary: true
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
cover:
    image: "/img/talks/elasticsearch-chatgpt/ashish_one_gpt.gif" # image path/url
    alt: "arch linux main screen" # alt text
    caption: "Arch linux login screen" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page

---

# Objective

In this hands-on workshop, We will learn how to connect ChatGPT to proprietary data stores using Elasticsearch and build question/answer capabilities for your data. In a demo, We will quickly convert your website, FAQ, or any documentation into prompt chat where your user can directly ask a question on your data. 

# Flow

![ChatGPT with Elasticsearch](/img/talks/elasticsearch-chatgpt/flow.png)

# Prerequisites

1. You have used ChatGPT :)

2. Good to have understanding around Elasticsearch (Not mandatory, Introduction will be cover)

3. System + Internet connection

4. Git - Install it from [https://git-scm.com/downloads](https://git-scm.com/downloads)

5. Docker - Install it from [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/).

6. Having basic python knowledge will be good.  

7. OpenAI account with API key - Create new one from [https://platform.openai.com/account/api-keys](https://platform.openai.com/account/api-keys). Make sure it having [free credits](https://platform.openai.com/account/usage). 

# 1. Setup cluster

1. Visit [cloud.elastic.co](https://cloud.elastic.co) and signup.

2. Create deployment

    1. Change settings according to your requirement or leave it default.

    2. Add machine learning instance - Simply click on "*__advance settings__*".

    3. Go to *__"Machine Learning instances" -> click on "Add Capacity"__* and select at least **2GB** ram capacity. 

    4. Finally click on "*__Create deployment__*". 

3. Download / Copy the deployment credentials.

4. Once deployment ready, click on "Continue". It will redirect you on kibana dashboard.

# 2. Upload third party Model

1. We are going to use [all-distilroberta-v1](https://huggingface.co/sentence-transformers/all-distilroberta-v1) model hosted on a hugging face. Lets import on an elastic cluster using eland.

2. We’re going to use docker for import model to the elastic cluster

    1. ```sh 
        git clone https://github.com/elastic/eland.git 
        cd eland
        ```
    2. ```sh 
        docker build -t elastic/eland .
        ```
    3. ```sh 
        docker run -it --rm elastic/eland eland_import_hub_model \
            --cloud-id <cloud_id> \
            -u elastic -p <elastic_cloud_password> \
            --hub-model-id sentence-transformers/all-distilroberta-v1 \
            --task-type text_embedding \
            --start

        ```
    4. Let's wait till the model gets uploaded without any error. 

    5. Exit from `eland` folder.
        ```sh
        cd ..
        ```

3. Go to the kibana panel. Navigate to *__Menu -> Analytics -> Machine Learning -> Trained model__*. You must see your model here in the “*__Started__*” state.

# 3. Crawling private data

1. Click on *__Menu -> Enterprise Search -> “Create an Elasticsearch index”__* button
2. Click on *__Web crawler__*.
3. Add index name (It will add prefix *__search__*) and  hit “*__Create index__*”. In my case index name is (search-ashish.one)
4. Go to “*__Pipelines__*” to create a pipeline. 
5. Click “*__Copy and customize__*” in the Ingest Pipeline Box.
6. Click “*__Add Inference Pipeline__*” in the Machine Learning Inference Pipelines box.
7. Give the unique pipeline name e.g. “*__ml-inference-ashish-one__*”
8. Select a trained ML Model from the dropdown “*__sentence-transformers__all-distilroberta-v1__*”
9. Select “*__title__*” as the Source field and set “*__title-vector__*” as a destination. You can specify your own destination field name.
10. Let's click on “*__Continue__*” and move to the Test(Optional) tab.  Click on “*__Continue__*” again.
11. At the Review stage let's click on “*__Create pipeline__*”.
12. Go to *__Menu -> Management -> Dev Tools__*. Let's create a mapping 

```sh
POST <index_name>/_mapping
{
  "properties": {
    "<vector_field_name>": {
      "type": "dense_vector",
      "dims": 768,
      "index": true,
      "similarity": "dot_product"
    }
  }
}
```

In my case mapping will be:

```sh
POST search-ashish.one/_mapping
{
  "properties": {
    "title-vector": {
      "type": "dense_vector",
      "dims": 768,
      "index": true,
      "similarity": "dot_product"
    }
  }
}
```

Paste above query in `cosole` and hit on play button.


13. Go to *__Enterprise search -> indices -> your_index_name -> Manage Domains__*. Enter the domain (e.g. https://ashish.one. You can add your own domain) to crawl and hit “*__Validate Domain__*”.
14. If everything is fine, simply click on “*__Add domain__*” and start crawling by click on *__Crawl -> Crawl all domains on this index__*.
15. Go to *__Enterprise Search -> Indices__*. You should see your index name. 

# 4. Setup Interface

## Get your credentials ready

1. `cloud_id` : Visit “*__[cloud.elastic.co](https://cloud.elastic.co)__*” -> Navigate to your deployment and click on “*__manage__*”. Simply copy Cloud ID and save it.
2. `cloud_user`: elastic
3. `cloud_password`: You will get it from step 1.3. If you forget to save, Simply click on *__“Action” -> “Reset password”__*. (Username will be elastic)
4. `openai_api`: Create open ai api key from [https://platform.openai.com/account/api-keys](https://platform.openai.com/account/api-keys).
5. `es_index`: Index name which we created in step 3.3. (search-ashish.one)
6. `vector_field`: The field which we've set for destination at step 3.9. i.e. **title-vector**


## Setup on local with Docker

1. Clone

```sh
git clone https://github.com/ashishtiwari1993/elasticsearch-chatgpt.git
cd elasticsearch-chatgpt
```

2. Replace credentials in `Dockerfile`

Open `Dockerfile` and change below creds

```sh
ENV openai_api="<open_api_key>"
ENV cloud_id="<elastic cloud id>"
ENV cloud_user="elastic"
ENV cloud_pass="<elastic_cloud_password>"
ENV es_index="<elasticsearch_index_name>"
ENV chat_title="<Any title for your page e.g. ashish.one GPT>"
ENV vector_field="< specify vector field where embedding will be save. e.g. title-vector>"
```

3. Build

```sh
docker build -t es-gpt .
```

4. Run

```sh
docker run -p 8501:8501 es-gpt
```

Simply visit on [localhost:8501](!http://localhost:8501)

## Setup on [Render](https://render.com/) with Docker

1. Signup on [https://render.com](https://render.com).

2. Create **Web Service**.

3. Go to **Public Git repository** section and add below repo url

```sh
https://github.com/ashishtiwari1993/elasticsearch-chatgpt
```

Hit on **Continue**.

4. Add **Name** and select **Free** Instance Type. 

5. Click on **Advanced** and **Add Environment Variable**

```sh
openai_api="<open_api_key>"
cloud_id="<elastic cloud id>"
cloud_user="elastic"
cloud_pass="<elastic_cloud_password>"
es_index="<elasticsearch_index_name>"                                                 
chat_title="<Any title for your page e.g. ashish.one GPT>"
vector_field="< specify vector field where embedding will be save. e.g. title-vector>"
```

6. Finally click on **Create Web Service**

## Output

![ashish.one ChatGPT](/img/talks/elasticsearch-chatgpt/ashish_one_gpt.gif)


# Reference
[Blog - ChatGPT and Elasticsearch: OpenAI meets private data](https://www.elastic.co/blog/chatgpt-elasticsearch-openai-meets-private-data)
