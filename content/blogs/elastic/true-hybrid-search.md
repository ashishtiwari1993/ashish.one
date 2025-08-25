---
title: 'Hybrid Search Done Right: Stop Calling Metadata Filters "Hybrid"'
date: 2025-08-25T12:09:27+05:30
draft: false
type: "post"
ogtype: "article"
tags: ["Search Relevancy","Hybrid Search","Semantic Search","Vector Search", "Elasticsearch","Information Retrieval","LLM"]
slug: "true-hybrid-search"
categories: ["Elastic"]
---

# Hybrid Search Done Right: Stop Calling Metadata Filters "Hybrid"

Everyone’s talking about **hybrid search** right now.
But here’s the uncomfortable truth:

👉 Just because you glued vector search onto your database and added metadata filters doesn’t mean you’ve built *true* hybrid search.

That’s like duct-taping a spoiler on a hatchback and calling it a race car. 🚗💨

Hybrid search is more than just “keyword + vector + filter.”
It’s about **field-level design, reranking, scoring, and scale**.

Let’s break this down.

---

## Field-Level Truth: Not Every Field Deserves Semantic Search

The biggest mistake I see:
Teams running semantic + lexical search on *every* field in their JSON docs.

That’s how you kill relevance.

### Lexical-Only Fields
These work best with exact match:
- `title`
- `name`
- `issue_id`
- `category`
- `tags`

If I search for `issue_id: 1245`, I want **1245** — not “similar looking IDs.”

### Semantic-Only Fields
These benefit from embeddings:
- `product_description`
- `movie_storyline`
- `customer_feedback`
- `reviews`

This is where meaning > keywords.
“Battery dies too quickly” should match with “poor battery life.”

### The Formula That Works
```text
Final Score = RRF(
   Lexical(title, name, category) +
   Semantic(description, feedback, reviews)
)
```

Hybrid works when **each field is treated for what it is**.

### Quick Reference Table

| Field Type            | Example Fields            | Best Search Method | Why? |
|------------------------|---------------------------|---------------------|------|
| Metadata / Identifiers | `id`, `issue_id`, `category` | Lexical             | Needs exact matching, filtering |
| Structured Short Text  | `title`, `name`, `tags`    | Lexical             | Precision > meaning |
| Unstructured Text      | `description`, `reviews`   | Semantic            | Context + meaning matter |

---

## Metadata Filters: Keep Them Lexical

One of the biggest anti-patterns: vectorizing filters.

Filters are not semantic. They should remain **exact**.

- Price → numeric filters
- Stock availability → boolean
- Categories, IDs → keyword
- Timestamps, geo → structured fields

Search is about *precision here*. If you fuzzy up filters with embeddings, you’ll tank user trust.

---

## Hybrid ≠ Metadata Filters + Vectors

Here’s the thing. Some vector DBs shout *“we support keyword search now!”*
But if you look closer, it’s often:
- A bolted-on library
- Or patched-in metadata filters

Yeah, technically it works.
But **that’s not hybrid**.

Search is not just filters.

---

## What Real Hybrid Search Looks Like

Let’s talk the real stuff beyond filters + vectors:

- **Autocomplete** — search-as-you-type, phrase suggesters, boosting, user-friendly completion.
- **Facets** — powerful aggregations: geo boundaries, date histograms, bucketing. Not just string counts.
- **Native rescoring** — query rescorer, rank features, boosting for freshness, popularity, personalization.
- **Rich documents** — nested JSON, arrays, geo points, IPs. Search isn’t flat.
- **Geo Search** — aggregations, polygons, proximity scoring. Beyond just “in radius.”
- **Access control** — index → document → field level security. For lexical **and** vector fields.
- **Data enrichment pipelines** — entity extraction, tagging, embeddings, LLM-based enrichment.
- **Scalability** — petabytes of data, tiered storage, lifecycle management. Search doesn’t stop at 10M docs.

This is hybrid.

---

## Vectors: Ask the Hard Questions

Not all vector support is equal. Check:

- Is vector search **native to the engine** or bolted on?
- Query types: KNN, ANN, hybrid filtering?
- Can it run at scale with good recall + low latency?
- Quantization: int4, int8, binary, advanced methods like **BBQ (Better Binary Quantization)**?
- Filters on HNSW: is it pre-filtering or filter-aware indexing (like ACORN)?
- Can it blend semantic + lexical scoring at query time?
   - RRF (Reciprocal Rank Fusion)
   - Linear retrievers
   - Semantic reranking with LLMs

If your engine can’t do these, it’s not serious about hybrid.

---

## Implementation Note: One Query Should Do It

If you need to stitch 3–4 services together just to get:
- Facets
- Filters
- Semantic results
- Lexical results
- Reranking

…you’re adding **latency + complexity**.

A true hybrid engine should give you everything in **one structured query**.

---

## Practical Takeaways

1. **Not all fields deserve semantic.** Treat fields differently.
2. **Filters are lexical.** Stop semantic-izing metadata.
3. **Hybrid = lexical + semantic + rescoring + scale.** Not just bolted-on keyword support.
4. **Evaluate vectors deeply.** Native support, quantization, filtering, reranking.
5. **One query > stitched services.** Hybrid done right = clean pipeline, low latency.

---

## Closing Thoughts

Hybrid search is not a checkbox.
It’s about designing for **relevance + scale + control**.

If your “hybrid” is just vectors + filters, you’ve built a demo — not a real search engine.

True hybrid is **Lexical + Semantic + Scoring + Access + Scale + Control**.
That’s when search feels natural, accurate, and production-ready.

---


