# Local RAG Playground

*A Microlearning Guide to Retrieval‑Augmented Generation*

This repository provides a **fully local, transparent playground** for learning how **Retrieval‑Augmented Generation (RAG)** works end‑to‑end. The goal is **understanding first**, not production scale. Everything runs locally. No cloud APIs. No black boxes. No vendor lock‑in.


## What This Project Is (and Is Not)

### ✅ This project **is**

*   A hands‑on learning environment for **RAG concepts**
*   A clean reference implementation
*   A safe place to experiment and break things
*   Fully local (data + models stay on your machine)

### ❌ This project is **not**

*   A production RAG system
*   A performance benchmark
*   A framework abstraction layer


## Core Technologies

This playground combines a small set of well‑understood components:

*   **Python** — pipeline logic and orchestration
    *   Entry point: `app/playground.ipynb`
*   **Ollama** — local LLM + embedding runtime
*   **PostgreSQL + pgvector** — vector storage and similarity search
*   **LangChain** — text splitting and retrieval helpers

Each piece is intentionally visible and modifiable.


## What You Will Learn

By working through this project, you will learn how to:

*   Ingest real documents (PDFs, resumes, notes)
*   Split text into **semantically meaningful chunks**
*   Generate **embedding vectors** locally
*   Store and query vectors using **pgvector**
*   Perform **semantic search**
*   Feed retrieved context into an LLM
*   Produce **grounded answers**, not hallucinations


## Project Structure

    env-llm-playground/
    ├── app/
    │   ├── playground.ipynb        # Main interactive entry point
    │   ├── chunker.py              # PDF text extraction & chunking
    │   ├── embedding_engine.py     # Calls Ollama embedding API
    │   ├── rag_controller.py       # Orchestrates RAG pipeline
    │   └── models.py               # SQLAlchemy ORM definitions
    ├── config/
    │   └── settings.py             # Ollama & PostgreSQL config
    ├── data/
    │   └── uploads/                # Input documents
    ├── docs/
    │   └── examples/               # Example queries & scenarios
    ├── readme-docker.md            # Docker environment setup
    ├── README.md                   # This file
    └── requirements.txt

***

## Component Responsibilities

| Component            | Responsibility                                    |
| -------------------- | ------------------------------------------------- |
| **Chunker**          | Extracts text and splits into semantic units      |
| **Embedding Engine** | Converts text to vectors using Ollama             |
| **pgvector**         | Stores vectors and performs similarity search     |
| **RAG Controller**   | Orchestrates ingestion, retrieval, and generation |
| **Ollama**           | Provides embeddings and LLM responses locally     |

Each component is deliberately small and readable.


## System Requirements (Practical Guidance)

### Hardware

*   **RAM:** 8 GB minimum (16 GB recommended)
*   **Disk:** \~10 GB free (models + vectors)
*   **CPU:** Modern multi‑core CPU

### Software

*   **Python 3.10+**
*   **Docker** (recommended for services)
*   **Ollama** (local model runtime)
*   **PostgreSQL 13+** with pgvector


## Intended Usage

This project works best as:

*   A **learning companion**
*   A **teaching example**
*   A **sandbox** for trying ideas before production
*   A **reference baseline** you can evolve


## Next Steps

Once you understand this playground, you’ll be well‑equipped to:

*   Scale to larger datasets
*   Add metadata filtering
*   Experiment with chunking strategies
*   Swap embedding models
*   Move to production systems with confidence
