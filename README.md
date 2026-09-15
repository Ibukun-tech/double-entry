# Double-Entry Bank Ledger

A production-grade double-entry accounting ledger API written in Go.

## Features

- Double-entry ledger model (accounts, entries, transfers)
- HTTP JSON API with JWT authentication
- PostgreSQL persistence + SQL migrations
- OpenAPI/Swagger docs
- Docker Compose for local development

## Quick Start (Docker)

1. Start services:

```bash
docker compose up -d
make createdb
make migrate-up-docker
```

2. Run the API locally (app built inside container):

```bash
make docker-up
```

The API will be available at http://localhost:8080 and Swagger UI at http://localhost:8080/swagger/

## Quick Start (Local Go)

Prerequisites: Go 1.26+, PostgreSQL (or use Docker Compose from above), `migrate`, and `sqlc` if you regenerate queries.

1. Configure environment variables (example `.env`):

```env
DB_URL=postgresql://root:secret@localhost:5432/simple_ledger?sslmode=disable
JWT_SECRET=your_jwt_secret_here
PORT=8080
CORS_ALLOWED_ORIGINS=http://localhost:3000
```

2. Run migrations against local DB:

```bash
make migrate-up-local
```

3. Start the server:

```bash
make server
```

## Environment Variables

- `DB_URL` - PostgreSQL connection string (preferred)
- `PORT` - HTTP server port (default `8080`)
- `JWT_SECRET` - Secret for signing JWT tokens (required for protected endpoints)
- `CORS_ALLOWED_ORIGINS` - Comma-separated origins for CORS

## Database Migrations

Migrations are stored in the `postgres/migrations/` directory. Use the Makefile targets:

- `make migrate-up-docker` — run migrations against Docker Postgres (default compose setup)
- `make migrate-up-local` — run migrations against local Postgres

## Development Commands

- `make sqlc` — regenerate SQL query bindings
- `make test` — run unit tests with race detector
- `make ci-test` — run tests with Docker-backed DB and migrations
- `make lint` — run linters

## API Documentation

Swagger JSON and UI are served at `/swagger/*` when the server is running. Example: http://localhost:8080/swagger/

## Project Structure (important paths)

- `cmd/` — application entrypoint
- `internal/api` — HTTP handlers and DTOs
- `internal/service` — business logic
- `internal/db` — database store and sqlc-generated code
- `postgres/migrations` — SQL migrations
