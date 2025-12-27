# Setup Docker Environement
Let’s walk it step by step, end to end, assuming you now have a `docker-compose.yml` with:

- `pgvector` (Postgres + pgvector)
- `pgadmin`
- `ollama` (local LLM)

## 1. Preconditions on your Windows machine
- Check Version:
  - `docker -v`
- Make sure:
  - Docker Desktop is installed.
  - WSL2 backend is enabled.
  - Docker is running (whale icon in system tray).

## 2. Start your stack
Open a terminal where your `docker-compose.yml` lives:

1. **Start all services in the background**
   ```bash
   docker compose up -d
   ```
2. **Verify containers are running**
   ```bash
   docker ps
   ```
   You should see something like:
   - `pgvector_db`
   - `pgadmin_ui`
   - `ollama_local` (or whatever names you used)


## 3. Configure PostgreSQL (pgvector)

1. **Open pgAdmin in your browser**
   - Go to: `http://localhost:5050`
   - Login with:
     - Email: `admin@admin.com`
     - Password: `admin`

2. **Register the Postgres server**
   - Right-click **Servers** → **Create** → **Server…**
   - **General → Name:** `pgvector_db` (any label you like)
   - **Connection tab:**
     - **Host name/address:** `pgvector`  
       (Use the Docker service name, not `localhost`.)
     - **Port:** `5432`
     - **Maintenance database:** `postgres`
     - **Username:** `postgres`
     - **Password:** `postgres`
   - Save.

3. **Enable the `vector` extension**
   - In pgAdmin, expand your server → **Databases → postgres**.
   - Right-click **postgres** → **Query Tool**.
   - Run:
     ```sql
     CREATE EXTENSION IF NOT EXISTS vector;
     ```
   - Execute (lightning bolt icon).

Now pgvector is ready.


## 4. Sanity check: simple table using `vector`

Run this in the same Query Tool to confirm everything works:

```sql
CREATE TABLE IF NOT EXISTS documents (
  id SERIAL PRIMARY KEY,
  content TEXT,
  embedding VECTOR(3)
);

INSERT INTO documents (content, embedding)
VALUES
  ('hello world', '[0.1, 0.2, 0.3]'),
  ('another doc', '[0.2, 0.1, 0.0]');
```

Then:

```sql
SELECT * FROM documents;
```

If that works, your DB + pgvector are fine.


## 5. Configure and test Ollama container (LLM)

1. **Enter the Ollama container**
   ```bash
   docker exec -it ollama_local bash
   ```
   (Replace `ollama_local` with your container_name if different.)

2. **Pull a model**
   Local Environment: - .env OLLAMA_MODEL

   ```bash
   ollama pull smollm:360m
   ```
   
   Live Environment: - .env OLLAMA_MODEL

   ```bash
   ollama pull llama3
   ```

3. **Exit the container**
   ```bash
   exit
   ```

4. **Test the API from your host (Windows)**
   In your PowerShell or terminal:

   ```bash
   curl http://localhost:11434/api/generate `
    -Method POST `
    -Body "{""model"":""smollm:360m"",""prompt"":""Hello from Windows via Docker""}" `
    -ContentType "application/json"
   ```

   You should see a streaming JSON response with generated text.


## 6. Stop and restart the whole stack

- **Stop all containers**
  ```bash
  docker compose down
  ```
- **Start again later**
  ```bash
  docker compose up -d
  ```

Volumes (`pgvector_data`, `pgadmin_data`, `ollama_models`) keep your data and models.
