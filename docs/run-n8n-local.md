# Run n8n locally (Docker Compose)

Prerequisites
- Docker & Docker Compose installed
- Copy `.env.example` → `.env` and fill the values

1) Start services
- Place `docker-compose.yml` in the repo root (see below)
- Run:
  docker compose up -d

2) Access n8n
- Open http://localhost:5678
- Log in (if you set auth) or start using the UI

3) Stop services
  docker compose down

Example `docker-compose.yml` (minimal)
```yaml name=docker-compose.yml
version: "3.8"
services:
  postgres:
    image: postgres:14
    environment:
      - POSTGRES_USER=${POSTGRES_USER}
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
      - POSTGRES_DB=${POSTGRES_DB}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  n8n:
    image: n8nio/n8n:latest
    environment:
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=${POSTGRES_DB}
      - DB_POSTGRESDB_USER=${POSTGRES_USER}
      - DB_POSTGRESDB_PASSWORD=${POSTGRES_PASSWORD}
      - N8N_BASIC_AUTH_ACTIVE=false
    ports:
      - "5678:5678"
    volumes:
      - ./.n8n:/home/node/.n8n
    depends_on:
      - postgres

volumes:
  postgres_data:
