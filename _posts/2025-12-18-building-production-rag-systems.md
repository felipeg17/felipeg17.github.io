---
layout: post
title: "Building Production RAG Systems: Lessons from 3 Years in Industry"
date: 2025-12-18
categories: [ai-ml, development]
tags: [python, rag, openai, chromadb, fastapi, genai]
excerpt: "Three years of building GenAI solutions taught me that RAG systems are 20% embeddings and 80% engineering. Here's what I learned taking RAG from prototype to production."
---

Over the past three years at Seguros Bolivar, I've built multiple RAG (Retrieval-Augmented Generation) systems for document analysis, claims processing, and automated research. What started as experimental prototypes have evolved into production systems handling thousands of queries monthly.

Here's what I learned about taking RAG from notebook to production.

## The Challenge: Moving Beyond the Demo

Building a RAG demo is straightforward:
1. Load documents
2. Generate embeddings
3. Store in vector database
4. Query and generate responses

But production systems require addressing questions the demos skip:
- How do you handle document updates without re-embedding everything?
- What happens when users ask questions outside your document scope?
- How do you monitor and improve relevance over time?
- How do you scale to handle concurrent users?

## Architecture That Actually Works

After several iterations, here's the architecture that's proven robust:

**Vector Store:** ChromaDB
We chose ChromaDB for its simplicity and Python-first API. Unlike Pinecone or Weaviate, ChromaDB runs locally for development and scales for production without infrastructure complexity.

**LLM:** OpenAI GPT-4 via API
While we experimented with open-source models, GPT-4's reliability and quality justified the cost for business-critical applications.

**API Layer:** FastAPI
FastAPI gives us:
- Async support for concurrent requests
- Automatic OpenAPI documentation
- Type safety with Pydantic models
- Easy deployment with Docker

**Deployment:** AWS EC2 with Docker
Simple but effective. We use EC2 for compute, RDS for metadata, and S3 for document storage.

## Key Lessons

### 1. Chunking Strategy Matters More Than You Think

We tested chunk sizes of 512, 1024, and 2048 tokens:
- **512 tokens**: Better precision, but missed context across sections
- **2048 tokens**: Better context, but noise reduced relevance
- **1024 tokens**: Sweet spot for our insurance documents

We also implemented **overlapping chunks** (128-token overlap) to prevent information loss at boundaries.

### 2. Prompt Engineering is Critical

Generic prompts like "Answer the question based on context" led to hallucinations and irrelevant responses.

What worked better:
```python
system_prompt = """You are an insurance document analyst.
Answer ONLY based on the provided context.
If the context doesn't contain the answer, say "I don't have enough information."
Be specific and cite relevant sections."""
```

Iterating on prompts improved accuracy by ~30%.

### 3. Hybrid Search Outperforms Pure Vector Search

Combining semantic search (embeddings) with keyword search (BM25) improved recall significantly:

```python
# Semantic search
semantic_results = chroma.query(embedding=query_embedding, n=10)

# Keyword search
keyword_results = bm25_search(query, n=10)

# Merge and rerank
final_results = rerank(semantic_results + keyword_results, query)
```

This hybrid approach caught both conceptual matches and specific terminology.

### 4. Monitoring is Essential

We track:
- **Query latency**: P50, P95, P99
- **Embedding quality**: Cosine similarity distributions
- **Relevance scores**: User feedback on answers
- **Cost per query**: OpenAI API usage

Without monitoring, you're flying blind.

### 5. User Feedback Loops Improve the System

We added a simple thumbs-up/thumbs-down on responses. Low-rated queries go into a review queue where we:
- Improve prompts
- Add missing documents
- Adjust chunking for specific doc types

## Code Example: Simple RAG Service

Here's a simplified version of our RAG API:

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
import chromadb
from openai import OpenAI

app = FastAPI()
client = OpenAI()
chroma_client = chromadb.Client()
collection = chroma_client.get_collection("documents")

class Query(BaseModel):
    question: str
    n_results: int = 5

@app.post("/query")
async def query_documents(query: Query):
    # Retrieve relevant chunks
    results = collection.query(
        query_texts=[query.question],
        n_results=query.n_results
    )

    # Build context
    context = "\n\n".join(results['documents'][0])

    # Generate response
    response = client.chat.completions.create(
        model="gpt-4",
        messages=[
            {"role": "system", "content": "Answer based on context only."},
            {"role": "user", "content": f"Context:\n{context}\n\nQuestion: {query.question}"}
        ]
    )

    return {
        "answer": response.choices[0].message.content,
        "sources": results['ids'][0]
    }
```

## Performance at Scale

Our production system handles:
- **500+ documents** in the vector store
- **2000+ queries/month**
- **<2 second** average response time
- **~$0.10** cost per query (OpenAI + infrastructure)

## What's Next?

We're exploring:
- **Reranking models** to improve retrieval quality
- **Agent-based RAG** for multi-step reasoning
- **Fine-tuned embeddings** for domain-specific terminology

## Resources

If you're building RAG systems, check out:
- **LangChain** for rapid prototyping
- **LlamaIndex** for advanced retrieval
- **ChromaDB docs** for vector store implementation
- **OpenAI Cookbook** for prompt patterns

## Conclusion

Production RAG systems require more engineering than AI. Focus on:
1. Robust document processing pipelines
2. Thoughtful chunking strategies
3. Comprehensive monitoring
4. Continuous improvement via user feedback

The 20% that is AI (embeddings, LLMs) gets the attention, but the 80% that is engineering (APIs, scaling, monitoring) makes the difference between a demo and a product.

Want to discuss RAG systems? [Get in touch](/contact/)!
