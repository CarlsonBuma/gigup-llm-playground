# ⏱️ 10‑Minute RAG Crash Course

*Everything you need to understand RAG — fast.*

This document explains the **core ideas**, **mental models**, and **workflow** behind Retrieval‑Augmented Generation (RAG). If you understand this file, you will understand **most modern AI assistants**, document search systems, and knowledge‑based chatbots. No theory overload. No framework noise. Just the essentials.


## 1. What Problem Does RAG Solve?

Large Language Models (LLMs) are powerful, but fundamentally limited:

*   They **hallucinate**
*   They **don’t know your private data**
*   They **can’t stay up‑to‑date**
*   They **struggle with long documents**
*   They **cannot store large knowledge bases internally**

RAG solves these problems by **separating knowledge from reasoning**.

The LLM no longer needs to *remember everything* — it only needs to **reason over retrieved information**.


## 2. What RAG Actually Is (Core Idea)

**RAG = Retrieval + Generation**

Instead of asking a model to answer from memory, you:

1.  **Retrieve** relevant information from your knowledge base
2.  **Generate** an answer using that information as context

Think of RAG as giving the model **read‑access to a searchable memory**.


### The Two Halves of RAG

#### ① Retrieval — *Finding the right information*

Search your document collection for the most relevant pieces related to the user’s question.

#### ② Generation — *Explaining it in natural language*

Feed those pieces to an LLM to generate a grounded, coherent answer.


### Why RAG Works Better

| Problem            | RAG Solution                           |
| ------------------ | -------------------------------------- |
| Hallucinations     | Answers are grounded in real documents |
| Outdated knowledge | You control what’s stored              |
| Black‑box answers  | You can inspect retrieved sources      |
| Privacy concerns   | Data stays local                       |
| Accuracy           | Answers are based on retrieved facts   |

## High‑Level RAG Architecture

### Ingestion Pipeline

    Document
      ↓
    Text Extraction
      ↓
    Semantic Chunking
      ↓
    Embedding Generation (Ollama)
      ↓
    Vector Storage (pgvector)


### Retrieval + Generation Pipeline

    User Query
      ↓
    Query Embedding
      ↓
    Vector Similarity Search
      ↓
    Top‑K Relevant Chunks
      ↓
    LLM Prompt Assembly
      ↓
    Grounded Answer


## Example Data Flow

    1. Upload resume.pdf
       → Extract text
       → Split into chunks (context‑aware)
       → Generate embeddings
       → Store in PostgreSQL

    2. Ask: "What are the candidate's key skills?"
       → Embed the question
       → Find similar chunks (cosine similarity)
       → Retrieve top matches
       → Generate answer using retrieved context

## 3. The RAG Loop (Minimal Version)

    User Question
       ↓
    Convert to embedding
       ↓
    Vector search (pgvector)
       ↓
    Retrieve top‑k chunks
       ↓
    Pass chunks + question to LLM
       ↓
    Grounded answer

Everything else in a RAG system is just **optimization around this loop**.


## 4. How Documents Become Searchable

Retrieval only works **after proper ingestion**.

### Step‑by‑Step

1.  **Extract text**  
    From PDFs, Markdown, HTML, etc.

2.  **Chunk the text**  
    Split into small, meaningful sections.

3.  **Generate embeddings**  
    Convert each chunk into a vector.

4.  **Store in pgvector**  
    Each chunk becomes searchable.

Each stored chunk contains:

*   chunk text
*   embedding vector
*   document reference
*   chunk index


## 5. What Are Embeddings?

Embeddings are **numeric representations of meaning**.

Similar meanings → vectors close together.

Example:

*   “software developer”
*   “software engineer”

These phrases are **semantically close**, even if the words differ.

That’s why embeddings enable **semantic search**, not keyword matching.


### Embedding Model Used

**`mxbai‑embed‑large`**

*   Multilingual
*   1024‑dimensional vectors
*   Well‑suited for documents, resumes, and knowledge bases


## 6. How Vector Search Works

When a user asks a question:

1.  Convert the question into an embedding
2.  Compare it to stored embeddings
3.  Measure similarity (e.g. cosine distance)
4.  Return the closest matches

This is how the system “understands” meaning without language.


## 7. Where the LLM Fits In (Important)

The LLM is **not** used for searching.

It is used **after retrieval**.

Inputs to the LLM:

*   the user’s question
*   the retrieved chunks

The LLM’s job:

*   summarize
*   explain
*   combine
*   reason

This dramatically reduces hallucinations.


## 8. Why Chunking Is Critical

Chunking quality directly affects retrieval quality.

*   Too large → embeddings become noisy
*   Too small → context is lost
*   Random splitting → poor retrieval

Your setup uses:

*   semantic splitting
*   overlap to preserve context
*   modern RAG‑friendly defaults

Chunking is often **more important than the LLM**.


## 9. What Actually Happens During a Query

Example question:

    "What skills does the candidate have?"

Process:

1.  Embed the question
2.  Vector search finds similar chunks
3.  Top‑k chunks are retrieved
4.  Chunks + question go to the LLM
5.  LLM generates a grounded answer

That’s the entire RAG system in motion.


## 10. RAG Mental Models (Remember These)

✅ RAG is **structured search + structured generation**  
✅ Embeddings are the heart of retrieval  
✅ The LLM is optional — retrieval alone is powerful  
✅ Local systems are easier to debug and trust  
✅ RAG is explainable: you can always inspect retrieved chunks


## Final Takeaway

If you remember only this, you understand RAG:

1.  Extract text
2.  Chunk text
3.  Embed chunks
4.  Store vectors
5.  Embed query
6.  Vector search
7.  Retrieve top‑k
8.  Feed to LLM
9.  Generate grounded answer
10. Repeat

That’s it.

Everything else is engineering.
