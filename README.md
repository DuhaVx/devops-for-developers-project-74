### CI Status
[![Actions Status](https://github.com/DuhaVx/devops-for-developers-project-74/actions/workflows/hexlet-check.yml/badge.svg)](https://github.com/DuhaVx/devops-for-developers-project-74/actions)
[![Push Workflow](https://github.com/DuhaVx/devops-for-developers-project-74/actions/workflows/push.yml/badge.svg)](https://github.com/DuhaVx/devops-for-developers-project-74/actions/workflows/push.yml)

## Project
Blog application packaged with Docker and Docker Compose.

Docker Hub image: `duhavx/devops-for-developers-project-74`

## System Requirements
- Docker 24+
- Docker Compose v2 (`docker compose`)
- GNU Make

## Environment Variables
Application is configured through environment variables:
- `DATABASE_HOST`
- `DATABASE_PORT`
- `DATABASE_NAME`
- `DATABASE_USERNAME`
- `DATABASE_PASSWORD`
- `NODE_ENV`

For local app development you can prepare `.env` in `app`:

```bash
cp app/.env.example app/.env
```

## Commands
Run from repository root:

```bash
make setup   # install dependencies inside container
make dev     # run development stack (app + caddy)
make test    # run tests in Docker (production compose)
make ci      # CI-like run in Docker
```

## Run Application
Development mode:

```bash
make dev
```

Application is available through Caddy on `http://localhost`.

## Run Tests
Tests are executed inside Docker containers:

```bash
make test
```

In CI, tests are also started in Docker via `make ci` (workflow `push.yml`).