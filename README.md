# L02

A small containerized Python application that serves a simple greeting page and tracks a visit counter in Redis.

## Overview

This project uses:

- FastAPI for the application API
- Redis for hit counting
- Nginx for serving a static frontend and proxying API traffic
- Docker Compose for local orchestration

The app is designed to run as a multi-container stack using Docker.

## Project structure

- `main.py` – FastAPI app with `/` and `/current` endpoints
- `requirements.txt` – Python dependencies
- `Dockerfile` – image definition for the FastAPI service
- `docker-compose.yml` – local orchestration for nginx, fast-api, and redis
- `nginx_default.conf` – Nginx reverse proxy configuration
- `www/index.html` – static landing page served by Nginx
- `.env` – environment variables for the Docker image tag

## Features

- `GET /` returns a greeting and increments a counter in Redis
- `GET /current` returns the current Redis hit count
- Nginx serves a static page on port `8000`
- Requests under `/api` are proxied to the FastAPI service

## Prerequisites

- Docker
- Docker Compose

## Configuration

The project uses `.env` to configure the Docker image tag used by Compose:

```env
TAG=v1
DOCKER_HUB=shinebo/l02:${TAG}
```

You can change `TAG` to create a different image tag if needed.

## Running locally

From the project root:

```bash
docker compose up --build
```

Then open:

- http://localhost:8000

The static page is served by Nginx, while the API endpoints are backed by the FastAPI app.

## API examples

```bash
curl http://localhost:8000/
curl http://localhost:8000/current
```

Example response from `/`:

```text
Hello 2222! I have been seen 1 times.
```

## Running without Docker

If you want to run the API directly:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn main:app --host 0.0.0.0 --reload
```

Then access the app at:

- http://localhost:8000

Note: the app expects Redis to be available at `redis:6379` in Docker-based deployments. For local non-Docker execution, update the Redis host configuration in `main.py` if needed.

## Dependencies

Python packages used:

- `fastapi`
- `redis`
- `uvicorn`

## Notes

This repository is a lightweight demo app for containerized service composition and basic Redis-backed counters. It is intended for local development and learning rather than production deployment.
