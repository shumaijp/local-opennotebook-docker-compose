# Local OpenNotebook Docker Compose

Run OpenNotebook locally with Ollama as the LLM provider — no external API required.

## Services

| Service | Description | Port |
|---------|-------------|------|
| [Ollama](https://ollama.ai/) | Local LLM server | 11434 |
| [OpenNotebook](https://github.com/lfnovo/open_notebook) | Notebook UI | 8502 |
| OpenNotebook API | Backend API | 5055 |

## Requirements

- Docker & Docker Compose
- **GPU version**: NVIDIA GPU + NVIDIA Container Toolkit
- **CPU version**: Works without GPU (LLM response will be slower)

## Quick Start

### 1. Start Services

```bash
# GPU version (requires NVIDIA GPU)
docker compose up -d

# CPU version
docker compose -f docker-compose-cpu.yml up -d
```

### 2. Install Models (via Ollama)

Available models can be found at [Ollama Library](https://ollama.ai/library).

#### Chat Model / Transformation Model

```
docker exec ollama-rag ollama pull gemma2:2b
```

Recommended setup:

| Model | Use Case | RAM |
|-------|----------|-----|
| gemma2:2b | Ultra-light (testing) | 4GB |
| gemma2:9b | Light, strong Japanese support (daily use) | 8GB |
| qwen2.5:14b | High performance, RAG-ready (document analysis) | 16GB+ |
| gpt-oss:20b | High performance, general purpose | 24GB+ |

#### Embedding Model

```
docker exec ollama-rag ollama pull mxbai-embed-large
```

### 3. Register Models in OpenNotebook

1. Open http://localhost:8502
2. Go to Settings → Models
3. Add:
    - Chat model: qwen2.5:7b
    - Embedding model: mxbai-embed-large

### 4. Start Using

Upload PDFs to a notebook and ask questions — processing is fully local.

### 5. Stop Services

```
docker compose down
```

To delete all stored data and models:

```
docker compose down -v
```

## File Structure

```
.
└── docker-compose.yml    # Main stack
```

Data is stored in Docker volumes:
- ollama-data
    - Ollama models and cache
- notebook-data
    - OpenNotebook files
- surreal-data
    - DB storage