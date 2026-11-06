<h1 align="center">Local Development Tools Stack</h1>

<h6 align="center">All essential development tools running locally at 127.0.0.1</h6>

## Introduction

These days, Iran is under heavy sanctions and many global services are not available.
This project provides a complete local stack of essential development tools that you can run on your localhost.

## Services Included

### API Development & Documentation
- **Swagger Editor** (Port 7880) - Design and document REST APIs
- **Swagger UI** (Port 7881) - Display and test API documentation
- **Redoc** (Port 8898) - Beautiful API documentation

### Data Science & Research
- **Jupyter Notebook** (Port 8888) - Interactive data science environment
- **RStudio** (Port 8894) - IDE for R programming (user: rstudio, pass: rstudio)

### Documentation & Writing
- **Overleaf** (Port 8889) - LaTeX editor for academic papers

### Development Tools
- **VS Code Server** (Port 8890) - VS Code in browser (pass: admin)
- **GitLab** (Port 8891) - Git repository management
- **Draw.io** (Port 8893) - Diagram and flowchart editor

### Database Management
- **pgAdmin** (Port 8895) - PostgreSQL management (email: admin@local.com, pass: admin)
- **Mongo Express** (Port 8896) - MongoDB management (user: admin, pass: admin)

### Cloud & Collaboration
- **NextCloud** (Port 8899) - File storage and sharing
- **Mattermost** (Port 8900) - Team communication platform

### Container Management
- **Portainer** (Port 9000) - Docker container management

### AI & Local LLMs
- **Ollama** (Port 11434) - Run AI models locally (OpenAI-compatible API)
- **Open WebUI** (Port 8901) - ChatGPT-like web interface for local models
- **Coding models** - `qwen2.5-coder:7b`, `qwen2.5-coder:1.5b` and `nomic-embed-text` are downloaded automatically on first start

## Quick Start

Run all services with one command:

```bash
docker compose up -d
```

## Service URLs

| Service | URL | Credentials |
|---------|-----|-------------|
| Swagger Editor | http://localhost:7880 | - |
| Swagger UI | http://localhost:7881 | - |
| Jupyter | http://localhost:8888 | Token in logs |
| Overleaf | http://localhost:8889 | Register first time |
| VS Code | http://localhost:8890 | Password: admin |
| GitLab | http://localhost:8891 | root / Set on first login |
| Draw.io | http://localhost:8893 | - |
| RStudio | http://localhost:8894 | rstudio / rstudio |
| pgAdmin | http://localhost:8895 | admin@local.com / admin |
| Mongo Express | http://localhost:8896 | admin / admin |
| NextCloud | http://localhost:8899 | Setup on first visit |
| Mattermost | http://localhost:8900 | Setup on first visit |
| Portainer | http://localhost:9000 | Setup on first visit |
| Redoc | http://localhost:8898 | - |
| Open WebUI | http://localhost:8901 | First signup becomes admin |
| Ollama API | http://localhost:11434 | - |

## Usage Tips

### Get Jupyter Token
```bash
docker logs jupyter-notebook 2>&1 | grep token
```

### Start Specific Services
```bash
docker compose up -d swagger-editor jupyter
```

### Stop All Services
```bash
docker compose down
```

### Remove All Data (Warning: This deletes all data!)
```bash
docker compose down -v
rm -rf *-data
```

## Local AI Coding Assistant (Ollama)

Everything runs on your machine — no internet or API key needed after the models are downloaded.

### Start only the AI services
```bash
docker compose up -d ollama ollama-models open-webui
docker logs -f ollama-models   # watch model download progress
```

### Choose which models to download
By default `qwen2.5-coder:7b` (chat), `qwen2.5-coder:1.5b` (autocomplete) and `nomic-embed-text` (codebase search) are pulled.
Override the list with the `CODING_MODELS` variable (space-separated), either in your shell or in a `.env` file:

```bash
CODING_MODELS="qwen2.5-coder:14b deepseek-coder-v2:16b" docker compose up -d ollama-models
```

Or pull any model manually at any time:
```bash
docker exec -it ollama ollama pull codellama:7b
docker exec -it ollama ollama list
```

### Recommended coding models

| Model | Size | RAM needed | Good for |
|-------|------|------------|----------|
| `qwen2.5-coder:1.5b` | ~1 GB | 4 GB | Fast autocomplete, weak machines |
| `qwen2.5-coder:7b` | ~4.7 GB | 8 GB | Best all-round choice |
| `qwen2.5-coder:14b` | ~9 GB | 16 GB | Better quality |
| `qwen2.5-coder:32b` | ~20 GB | 32 GB+ | Near cloud-level quality |
| `deepseek-coder-v2:16b` | ~9 GB | 16 GB | Strong multi-language coding |
| `qwen3-coder:30b` | ~19 GB | 32 GB+ | Agentic coding, large context |
| `codellama:7b` | ~3.8 GB | 8 GB | Classic code model |
| `starcoder2:3b` | ~1.7 GB | 4 GB | Lightweight autocomplete |

Browse more at https://ollama.com/library?c=code

### Chat from the terminal
```bash
docker exec -it ollama ollama run qwen2.5-coder:7b
```

### Use it from your code (OpenAI-compatible API)
```bash
curl http://localhost:11434/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model": "qwen2.5-coder:7b", "messages": [{"role": "user", "content": "Write a Python function to reverse a string"}]}'
```
Any tool that supports OpenAI can use it: set the base URL to `http://localhost:11434/v1` and use any value as the API key.

### Use it inside VS Code (Copilot-like)
Install the **Continue** extension (works in desktop VS Code and in the bundled VS Code Server at port 8890), then put this in `~/.continue/config.yaml`:

```yaml
name: Local Assistant
version: 1.0.0
schema: v1
models:
  - name: Qwen2.5 Coder 7B
    provider: ollama
    model: qwen2.5-coder:7b
    apiBase: http://localhost:11434   # inside code-server use http://ollama:11434
    roles: [chat, edit, apply]
  - name: Qwen2.5 Coder 1.5B
    provider: ollama
    model: qwen2.5-coder:1.5b
    apiBase: http://localhost:11434
    roles: [autocomplete]
  - name: Nomic Embed
    provider: ollama
    model: nomic-embed-text
    apiBase: http://localhost:11434
    roles: [embed]
```

> Note: for `http://ollama:11434` to work from code-server, both containers run in the same compose network (they do by default).

### Use it from the terminal with Aider
```bash
pip install aider-chat
export OLLAMA_API_BASE=http://localhost:11434
aider --model ollama_chat/qwen2.5-coder:7b
```

### Performance notes
- **macOS (Apple Silicon):** Docker cannot use the Mac GPU, so Ollama in Docker runs on CPU only. For much faster responses install Ollama natively (`brew install ollama` or https://ollama.com/download) and stop the `ollama` container — everything above keeps working on `localhost:11434`.
- **Linux + NVIDIA GPU:** install the NVIDIA Container Toolkit and uncomment the `deploy:` section of the `ollama` service in `docker-compose.yml`.
- Models are stored in `./ollama-data`, so they are downloaded only once.

## Data Persistence

All services store their data in local directories:
- `./jupyter-data` - Jupyter notebooks
- `./overleaf-data` - LaTeX projects
- `./code-server-data` - VS Code projects
- `./gitlab-data` - Git repositories
- `./nextcloud-data` - Cloud files
- `./ollama-data` - Downloaded AI models
- `./open-webui-data` - Open WebUI chats and settings
- And more...

## Resource Requirements

**Minimum:**
- RAM: 8GB
- Disk: 20GB free space
- Docker & Docker Compose installed

**Recommended:**
- RAM: 16GB+
- Disk: 50GB+ free space
- SSD for better performance

**For AI models:** add the model's RAM requirement (see table above) and ~5-20 GB disk per model.

## Customization

Edit `docker-compose.yml` to:
- Change ports
- Modify passwords
- Add/remove services
- Configure resource limits


