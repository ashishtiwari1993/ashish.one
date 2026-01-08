---
title: "Streamlining Vector Search with private model: Unifying Embedding Models with LiteLLM and Elasticsearch"
date: 2026-01-08T21:21:21+05:30
draft: false
type: "post"
tags: ["elasticsearch", "vector search", "litellm", "ai", "docker", "embeddings", "elastic"]
categories: ["Elastic"]
description: "Learn how to host an embedding model using LiteLLM proxy on Docker and consume it directly from Elasticsearch for seamless vector search."
cover:
    image: "/img/elastic/litellm-elasticsearch.png" # image path/url
    alt: "LiteLLM with Elasticsearch" # alt text
    relative: true # when using page bundles set this to true
    hidden: true # only hide on current single page

---

In the rapidly evolving landscape of AI, managing the "plumbing" between your embedding models and your search engine is often a challenge. Developers frequently struggle with switching providers, managing API keys, and maintaining consistent API specifications.

**LiteLLM** solves the model management problem by acting as a universal proxy, while **Elasticsearch** delivers high-performance Vector Search. By combining them, you can build a search architecture that is both flexible and powerful.

In this guide, we will walk through hosting an OpenAI-compatible embedding model using LiteLLM on Docker and consuming it directly from Elasticsearch to perform seamless vector search.

### Prerequisites

* **OS:** Ubuntu 24.04.3 LTS (or similar Linux environment)
* **Docker:** Ensure Docker is installed and running.
* **Elasticsearch:** An Elastic Cloud instance or a self-managed cluster.

---

### Step 1: Configure and Run LiteLLM

First, we need to configure LiteLLM. This tool will act as our gateway, proxying requests to various models (like GPT-4 or specific embedding models) while presenting a unified OpenAI-compatible API to the outside world.

**1. Create the `config.yaml` file:**

```yaml
model_list:
  - model_name: gpt-4o
    litellm_params:
      model: openai/gpt-4o
      api_key: os.environ/OPENAI_API_KEY

  - model_name: gpt-3.5-turbo
    litellm_params:
      model: openai/gpt-3.5-turbo
      api_key: os.environ/OPENAI_API_KEY

  - model_name: text-embedding-3-small
    litellm_params:
      model: openai/text-embedding-3-small
      api_key: os.environ/OPENAI_API_KEY
```

**2. Run LiteLLM with Docker:**

We will run the container with the config mounted. We also define a `LITELLM_MASTER_KEY` (`sk-my-admin-key-123`), which will serve as the static API key for our internal services to authenticate against this proxy.

```bash
docker run -d \
  -p 4000:4000 \
  -v $(pwd)/config.yaml:/app/config.yaml \
  -e OPENAI_API_KEY="enter your openai api key" \
  -e LITELLM_MASTER_KEY="sk-my-admin-key-123" \
  docker.litellm.ai/berriai/litellm:main-latest \
  --config /app/config.yaml --detailed_debug
```

---

### Step 2: Verify the Proxy

Before we involve the search engine, let’s ensure our LiteLLM proxy is correctly handling requests.

**Test Chat Completion:**

```bash
curl [http://0.0.0.0:4000/v1/chat/completions](http://0.0.0.0:4000/v1/chat/completions) \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer sk-my-admin-key-123" \
  -d '{
    "model": "gpt-4o",
    "messages": [
      { "role": "user", "content": "Hello from LiteLLM Docker!" }
    ]
  }'
```

**Test Embedding Generation:**

```bash
curl [http://0.0.0.0:4000/v1/embeddings](http://0.0.0.0:4000/v1/embeddings) \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer sk-my-admin-key-123" \
  -d '{
    "model": "text-embedding-3-small",
    "input": "LiteLLM makes proxying easy."
  }'
```

---

### Step 3: Configure Elasticsearch Inference

Now we connect the two pieces. Since LiteLLM provides an OpenAI-compatible interface, we can use the standard OpenAI connector in Elasticsearch but point it to our custom LiteLLM URL.

*For more details on configuring these connectors, refer to the [API doc](https://www.elastic.co/docs/api/doc/elasticsearch/operation/operation-inference-put-openai)*

**Create the Inference Endpoint**

Run the following in your Kibana Dev Tools. Note that the `url` points to our container and the `api_key` matches our LiteLLM master key.

```json
PUT _inference/text_embedding/litellm-embeddings
{
   "service": "openai",
   "service_settings": {
       "api_key": "sk-my-admin-key-123",
       "model_id": "text-embedding-3-small",
       "url": "http://litellm_endpoint:4000/v1/embeddings",
       "dimensions": 128
   }
}
```

**Test the Inference Endpoint**

Let's verify that Elasticsearch can generate embeddings via LiteLLM:

```json
POST _inference/text_embedding/litellm-embeddings
{
 "input": "The sky above the port was the color of television tuned to a dead channel."
}
```

**Output:**

```json
{
 "text_embedding": [
   {
     "embedding": [
       -0.023704717,
       -0.016358491,
       ...
       0.12764767
     ]
   }
 ]
}
```

---

### Step 4: End-to-End Vector Search

Now that the pipeline is ready, we can perform **Semantic Search**. We will use the `semantic_text` field type, which abstracts the embedding generation process—Elasticsearch calls LiteLLM automatically during data ingestion and query time.

**1. Create the Index**

```json
PUT /litellm-test-index
{
 "mappings": {
   "properties": {
     "content": {
       "type": "semantic_text",
       "inference_id": "litellm-embeddings"
     },
     "category": {
       "type": "keyword"
     }
   }
 }
}
```

**2. Ingest Data**

```json
POST /litellm-test-index/_bulk
{ "index": {} }
{ "content": "The quick brown fox jumps over the lazy dog.", "category": "animals" }
{ "index": {} }
{ "content": "Artificial intelligence is transforming software development.", "category": "tech" }
{ "index": {} }
{ "content": "Docker containers make deployment consistent and easy.", "category": "tech" }
```

**3. Verify Data**

You can now search the index.

```json
GET litellm-test-index/_search
```

*Note: You will not see the embedding vectors in the `_source` output. This is by design to save storage space. To learn more about this behavior, check out the [Elastic Search Labs blog on excluding vectors from source](https://www.elastic.co/search-labs/blog/elasticsearch-exclude-vectors-from-source).*

**4. Perform Semantic Query**

Finally, let's run a natural language search. The query "tools for deploying software" does not exist in our text, but the vector search identifies the semantic relationship with "Docker containers."

```json
GET /litellm-test-index/_search
{
 "query": {
   "semantic": {
     "field": "content",
     "query": "tools for deploying software"
   }
 }
}
```

### Conclusion

This architecture demonstrates the power of decoupling your model provider from your search engine. By using **LiteLLM** as a standardized proxy, you gain the flexibility to swap underlying models without breaking your application. Simultaneously, **Elasticsearch's Vector Search** leverages these embeddings to provide deep semantic understanding.

Together, LiteLLM and Elasticsearch create a robust, future-proof foundation for building intelligent search applications that go far beyond simple keyword matching.

Happy searching!


