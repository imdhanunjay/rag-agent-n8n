# Run n8n locally (Docker Compose)

Purpose
This file shows a simple Docker Compose setup to run n8n and a Postgres database locally for development and testing of this workflow.

Prerequisites
- Docker and Docker Compose installed
- Copy `.env.example` to `.env` in the repo root and fill values (do NOT commit `.env` with real secrets)

Example docker-compose.yml
Save this file as `docker-compose.yml` in the repository root (or use the one already provided):

```yaml
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
