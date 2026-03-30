### CI Status
[![Actions Status](https://github.com/DuhaVx/devops-for-developers-project-74/actions/workflows/hexlet-check.yml/badge.svg)](https://github.com/DuhaVx/devops-for-developers-project-74/actions)
[![Push Workflow](https://github.com/DuhaVx/devops-for-developers-project-74/actions/workflows/push.yml/badge.svg)](https://github.com/DuhaVx/devops-for-developers-project-74/actions/workflows/push.yml)

## Project
Blog application packaged with Docker and Docker Compose.

**Docker Hub image:** `duhavx/devops-for-developers-project-74:latest`

```bash
docker pull duhavx/devops-for-developers-project-74:latest
docker run -p 8080:8080 -e NODE_ENV=development duhavx/devops-for-developers-project-74:latest make dev
```

## System requirements
- Docker 24+ and Docker Compose v2 (`docker compose`, or `docker-compose` ≥ 1.27)
- GNU Make
- Node.js ≥ 20 (optional, for editing the app outside containers)

## Environment variables
The app reads database settings from the environment (see `app/.env.example`).

For Docker Compose at the repo root, create a `.env` file (not committed), for example:

```env
DATABASE_HOST=db
DATABASE_NAME=postgres
DATABASE_USERNAME=postgres
DATABASE_PASSWORD=password
DATABASE_PORT=5432
```

You can copy the template and adjust:

```bash
cp app/.env.example .env
```

Variables include `DATABASE_HOST`, `DATABASE_PORT`, `DATABASE_NAME`, `DATABASE_USERNAME`, `DATABASE_PASSWORD`, and `NODE_ENV`.

## Makefile (from repository root)
| Command | Purpose |
|--------|---------|
| `make setup` | Install dependencies and run migrations inside the `app` container |
| `make dev` | Development stack (app, PostgreSQL, Caddy) |
| `make test` | Tests using `docker-compose.yml` only |
| `make ci` | Same as CI: `docker compose -f docker-compose.yml up --abort-on-container-exit --exit-code-from app` |

## Run application (development)
```bash
make dev
```

Open `https://localhost` (Caddy, self-signed TLS) or `http://localhost` (redirects to HTTPS).

## Run tests
Tests run inside Docker:

```bash
make test
```

CI runs `make ci` in `push.yml`.

## Docker layout
- **`Dockerfile`** — Node 20.12.2, `WORKDIR /app`, no copying of sources (used by dev override).
- **`Dockerfile.production`** — install deps, build, production image.
- **`docker-compose.yml`** — tests and production build; `app` has no published ports; `db` is PostgreSQL with a healthcheck.
- **`docker-compose.override.yml`** — dev: bind-mount `app/`, Caddy on 80/443.
