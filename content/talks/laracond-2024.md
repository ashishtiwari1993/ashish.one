---
title: "Laracon 2024"
date: 2025-08-25T11:29:16+05:30
draft: false
type: "post"
slug: "laracon-2024"
tags: ["elasticsearch","genai","RAG","vector-search","semantic-search","LLMs","PHP","Laravel"]
categories: ["Elastic"]
description: "Laracon India 2024 talk on building no-code RAG chatbots with PHP, LLMs, and Elasticsearch using the Elephant PHP library."
cover:
    image: "/img/misc/laracon-2024.jpg" # image path/url
    alt: "Generative AI and LLM in PHP | Ashish Tiwari | Laracon India 2025" # alt text
    caption: "Laracon India 2025" # display caption under cover
    relative: true # when using page bundles set this to true
    hidden: true # only hide on current single page

---

# 🎤 Talk Summary: No-Code RAG Chatbot with PHP, LLMs & Elasticsearch
**Speaker:** Ashish Diwali (Senior Developer Advocate, Elastic)

---

## 🔑 Introduction
- Topic: Integrating **Generative AI (LLMs)** with **PHP**.
- Goal: Show how to build **chat assistants, semantic search, and vector search** without heavy ML expertise.
- Demo focus: Using **Elasticsearch + PHP + LLM (LLaMA 3.1)**.

---

## 🧩 Core Concepts

### 1. Prompt Engineering
- LLMs generate responses based on prompts → predicting next words.
- Techniques:
  - **Zero-shot inference** → direct classification or tagging.
  - **One-shot inference** → provide one example in the prompt.
  - **Few-shot inference** → multiple examples → useful for structured outputs (SQL, JSON, XML).
- Iteration + context = **In-context learning (ICL)**.

### 2. LLM Limitations
- ❌ Hallucinations (wrong answers).
- ❌ Complex to build/train from scratch.
- ❌ No real-time / private data access.
- ❌ Privacy & security concerns (especially in banking, public sector).

### 3. RAG (Retrieval-Augmented Generation)
- Solution to limitations.
- Workflow:
  1. User query → hits database/vector DB (e.g., Elasticsearch).
  2. Retrieve **top 5–10 relevant docs**.
  3. Pass as context window → LLM generates accurate answer.
- Benefits:
  - Grounded responses.
  - Works with private data.
  - Avoids retraining large models.

---

## 🔍 Semantic & Vector Search
- **Semantic Search:** Understands meaning, not just keywords.
  - Example: “best city” ↔ “beautiful city.”
- **Vector Search:** Text, images, and audio converted into embeddings (arrays of floats).
  - Enables image search, recommendation systems, music search (via humming).
- **Similarity algorithms:** cosine similarity, dot product, nearest neighbors.

---

## 🛠️ Tools & Demo

### Elephant Library (PHP)
- Open-source PHP library for GenAI apps.
- Supports:
  - LLMs: OpenAI, Mistral, Anthropic, LLaMA.
  - Vector DBs: Elasticsearch, Pinecone, Chroma, etc.
  - Features: document chunking, embedding generation, semantic retrieval, Q&A (RAG).

### Demo Flow
1. **Ingestion**:
   - Chunk PDF into smaller pieces (800 chars).
   - Generate embeddings with LLaMA.
   - Store text + vectors in Elasticsearch.

2. **Querying**:
   - User question → hits Elasticsearch.
   - Retrieve top 10 docs.
   - Send docs + query → LLaMA → response.

3. **Examples**:
   - *“Who won the Nobel Prize in Physics 2024?”* → Retrieved correct answer from PDF context.
   - *“How do brain neural networks work?”* → Summarized based on provided docs.
   - *“Who won ICC Championship 2025?”* → No irrelevant hallucination (kept within context).

---

## 🎯 Key Takeaways
- **Don’t train your own LLM** → use **RAG + search** to build assistants on private data.
- **Elasticsearch** is a powerful vector DB for semantic + hybrid search.
- **PHP + Elephant** makes building RAG chatbots accessible for web developers.
- RAG powers most modern chat assistants today.

---


{{< youtube qDFct7oeRss >}}
