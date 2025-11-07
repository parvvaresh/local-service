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

## Data Persistence

All services store their data in local directories:
- `./jupyter-data` - Jupyter notebooks
- `./overleaf-data` - LaTeX projects
- `./code-server-data` - VS Code projects
- `./gitlab-data` - Git repositories
- `./nextcloud-data` - Cloud files
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

## Customization

Edit `docker-compose.yml` to:
- Change ports
- Modify passwords
- Add/remove services
- Configure resource limits


