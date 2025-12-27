# Project Overview
A Python project with Jupyter notebooks, leveraging PostgreSQL with pgVector, Ollama LLM runtime, and various data science and AI libraries.



### Prerequisites

- Python 3.8+ installed locally
- Docker Environment running (for database and LLM services)
- VS Code with Jupyter extension enabled


> **⚠️ System Requirements**: 
Even small LLM models (e.g., smollm:360m) run efficiently on modern CPUs, but they still benefit from having enough RAM and available compute. For a smooth experience when running Ollama inside Docker, ensure your machine has at least 8 GB of RAM, though 16 GB or more is recommended if you plan to run multiple services or larger models.

## Getting Started

### 1. Set Up Python Virtual Environment

```bash
# Create virtual environment
python -m venv venv

# Activate environment (Windows)
.\venv\Scripts\activate

# Activate environment (macOS/Linux)
source venv/bin/activate
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Start Docker Services

```bash
docker-compose up -d
```

See [readme-docker.md](readme-docker.md) for detailed service configuration and troubleshooting.

### 4. Deactivate Environment

```bash
deactivate
```

## Project Structure

```
├── app/
│   ├── init.ipynb          # Jupyter notebook initialization
│   ├── db/
│   │   ├── init.sql        # Database initialization script
│   │   └── extensions/
│   │       └── vector.sql  # pgVector extension setup
│   └── store/              # Storage directory
├── docker-compose.yml      # Docker services configuration
├── requirements.txt        # Python dependencies
├── readme-docker.md        # Setup Docker Environment
└── README.md               # This file
```

## Key Technologies

- **PostgreSQL + pgVector**: Vector database for embeddings
- **Ollama**: Local LLM runtime for inference
- **Jupyter**: Interactive notebooks for development
- **LangChain**: LLM orchestration and chains
- **GeoPandas/Folium**: Geospatial analysis and mapping
- **FastAPI**: Web API framework
- **SQLAlchemy**: Database ORM

## Quick Commands

| Command | Purpose |
|---------|---------|
| `.\venv\Scripts\activate` | Activate virtual environment (Windows) |
| `source venv/bin/activate` | Activate virtual environment (macOS/Linux) |
| `pip install -r requirements.txt` | Install Python dependencies |
| `docker-compose up -d` | Start Docker services |
| `docker-compose down` | Stop Docker services |
| `docker-compose logs -f` | View service logs |
| `deactivate` | Deactivate virtual environment |


## Resources

- See [readme-docker.md](readme-docker.md) for Docker setup and service documentation
- Notebooks are located in `app/*`
