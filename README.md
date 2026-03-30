### Статус CI
[![Actions Status](https://github.com/DuhaVx/devops-for-developers-project-74/actions/workflows/hexlet-check.yml/badge.svg)](https://github.com/DuhaVx/devops-for-developers-project-74/actions)
[![Push Workflow](https://github.com/DuhaVx/devops-for-developers-project-74/actions/workflows/push.yml/badge.svg)](https://github.com/DuhaVx/devops-for-developers-project-74/actions/workflows/push.yml)

## О проекте
Блог на Fastify, упакованный в Docker и Docker Compose.

**Образ на Docker Hub:** `duhavx/devops-for-developers-project-74:latest`

```bash
docker pull duhavx/devops-for-developers-project-74:latest
docker run -p 8080:8080 -e NODE_ENV=development duhavx/devops-for-developers-project-74:latest make dev
```

## Требования к системе
- Docker 24+ и Docker Compose v2 (`docker compose` либо классический `docker-compose` ≥ 1.27)
- GNU Make
- Node.js ≥ 20 (по желанию, для работы с кодом вне контейнеров)

## Переменные окружения
Настройки БД задаются через окружение (см. `app/.env.example`).

Для запуска через Docker Compose в **корне репозитория** создайте файл `.env` (в git не коммитится), например:

```env
DATABASE_HOST=db
DATABASE_NAME=postgres
DATABASE_USERNAME=postgres
DATABASE_PASSWORD=password
DATABASE_PORT=5432
```

Шаблон можно скопировать и отредактировать:

```bash
cp app/.env.example .env
```

Используются в том числе `DATABASE_HOST`, `DATABASE_PORT`, `DATABASE_NAME`, `DATABASE_USERNAME`, `DATABASE_PASSWORD`, а также `NODE_ENV`.

## Команды Makefile (из корня репозитория)
| Команда | Назначение |
|--------|------------|
| `make setup` | Установка зависимостей и миграции в контейнере `app` |
| `make dev` | Стек для разработки: приложение, PostgreSQL, Caddy |
| `make test` | Тесты только по `docker-compose.yml` |
| `make ci` | Как в CI: `docker compose -f docker-compose.yml up --abort-on-container-exit --exit-code-from app` |

## Запуск приложения (разработка)
```bash
make dev
```

В браузере: `https://localhost` (Caddy, самоподписной сертификат) или `http://localhost` (редирект на HTTPS).

## Запуск тестов
Тесты выполняются внутри Docker:

```bash
make test
```

В CI вызывается `make ci` (workflow `push.yml`).

## Структура Docker
- **`Dockerfile`** — образ Node 20.12.2, `WORKDIR /app`, без копирования исходников (для dev-override).
- **`Dockerfile.production`** — установка зависимостей, сборка, production-образ.
- **`docker-compose.yml`** — тесты и production-сборка; у `app` нет проброшенных портов; сервис `db` — PostgreSQL с healthcheck.
- **`docker-compose.override.yml`** — разработка: монтирование `app/`, Caddy на портах 80 и 443.
