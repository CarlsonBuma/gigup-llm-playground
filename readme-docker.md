# Docker Environment Setup (Local RAG Stack)

This document describes how to start and operate the **local Docker environment** used for experimentation with LLMs, embeddings, and vector databases.

## What This Stack Provides

The Docker stack includes:

*   **PostgreSQL + pgvector** — vector database
*   **pgAdmin** — database UI
*   **Ollama** — local LLM & embedding runtime
*   **Optional Open‑WebUI** — browser‑based chat UI

All services run on a **single internal Docker network** and communicate using **service names** (not `localhost`).

## Prerequisites

*   Docker Desktop installed
*   WSL2 backend enabled (Windows)
*   Docker running

Verify:

```bash
docker -v
docker compose version
```

## Starting the Environment

From the directory containing `docker-compose.yml`:

```bash
docker compose up -d
```

Verify containers:

```bash
docker ps
```

Expected containers include:

*   `pgvector_db`
*   `pgadmin_ui`
*   `ollama_local`
*   `ollama_model_init`
*   `open-webui` (if enabled)


## Automatic Initialization

### PostgreSQL / pgvector

*   PostgreSQL uses the `ankane/pgvector` image
*   The **pgvector extension is enabled automatically**
*   Initialization runs **only on first startup**, when the database volume is empty

You do **not** need to:

*   Open pgAdmin
*   Run `CREATE EXTENSION` manually

If volumes are removed (`docker compose down -v`), initialization will run again.


### Ollama Models (Automatic)

On first startup, a **one‑time init container** pulls a predefined set of models and then exits.

Example default models:

*   Lightweight LLM (e.g. `smollm:360m`)
*   Embedding model (e.g. `mxbai-embed-large`, 1024‑dim)

Models are stored in a Docker volume and persist across restarts.


## Pulling Additional Models (Optional)

Users can pull **additional or alternative models** at any time.

Enter the Ollama container:

```bash
docker exec -it ollama_local /bin/sh
```

Pull any supported model:

```bash
ollama pull llama3
ollama pull mistral
```

Verify available models:

```bash
ollama list
```

Exit:

```bash
exit
```

No restart is required — newly pulled models become immediately available.


## Service Access

### pgAdmin

*   URL: <http://localhost:5050>
*   Login: `admin@admin.com / admin`

When adding a server:

*   **Host:** `pgvector`
*   **Port:** `5432`

### Ollama API

*   From host: `http://localhost:11434`
*   From containers: `http://ollama:11434`

### Open‑WebUI (if enabled)

*   URL: <http://localhost:3000>


## Stopping or Resetting the Environment

Stop containers:

```bash
docker compose down
```

Restart later:

```bash
docker compose up -d
```

Full reset (⚠ removes database & models):

```bash
docker compose down -v
```


## Key Notes

*   Containers **never use localhost** to talk to each other
*   Service names act as internal DNS
*   Initialization steps are **idempotent**
*   Volumes ensure persistence
*   The setup is intended as a **local playground environment**
