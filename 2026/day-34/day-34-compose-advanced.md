# Day 34 – Docker Compose Advanced

## Overview
Multi-container production-like applications using Docker Compose.

Includes:
- Python + Postgres + Redis
- Node + React + Mongo + Redis
- Multi-stage Docker builds
- Healthchecks
- Networks
- Volumes

---

## Python Stack

### docker-compose.yml

```yaml
services:

  web:
    build: ./app
    ports:
      - "5000:5000"

    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_healthy

    networks:
      - python-app-network

  db:
    image: postgres:16

    environment:
      POSTGRES_PASSWORD: password

    volumes:
      - postgres-data-python:/var/lib/postgresql/data

    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 3s
      retries: 5

    networks:
      - python-app-network

  redis:
    image: redis:latest

    healthcheck:
      test: ["CMD","redis-cli","ping"]
      interval: 5s
      timeout: 3s
      retries: 5

    networks:
      - python-app-network

networks:
  python-app-network:

volumes:
  postgres-data-python:
```

---

### Python Dockerfile

```dockerfile
FROM python:3.11 AS builder

WORKDIR /app

COPY requirements.txt .

RUN pip install --user -r requirements.txt

FROM gcr.io/distroless/python3-debian12

WORKDIR /app

COPY --from=builder /root/.local /root/.local

COPY . .

ENV PATH=/root/.local/bin:$PATH

EXPOSE 5000

CMD ["app.py"]
```

---

## Node Fullstack Stack

### docker-compose.yml

```yaml
services:

  frontend:
    build: ./frontend
    ports:
      - "80:80"

    depends_on:
      backend:
        condition: service_healthy

    networks:
      - node-app-network

  backend:
    build: ./backend

    ports:
      - "3000:3000"

    depends_on:
      mongo:
        condition: service_healthy
      redis:
        condition: service_healthy

    networks:
      - node-app-network

    healthcheck:
      test: ["CMD","curl","-f","http://localhost:3000"]
      interval: 5s
      timeout: 3s
      retries: 5

  mongo:
    image: mongo:7

    volumes:
      - mongo-data:/data/db

    networks:
      - node-app-network

    healthcheck:
      test: ["CMD","mongosh","--eval","db.runCommand({ ping:1 })"]
      interval: 10s
      timeout: 5s
      retries: 5

  redis:
    image: redis:7

    networks:
      - node-app-network

    healthcheck:
      test: ["CMD","redis-cli","ping"]

networks:
  node-app-network:

volumes:
  mongo-data:
```

---

## Backend Dockerfile

```dockerfile
FROM node:20 AS builder

WORKDIR /app

COPY package*.json .

RUN npm install --only=production

FROM node:20-slim

WORKDIR /app

COPY --from=builder /app/node_modules ./node_modules

COPY . .

EXPOSE 3000

CMD ["node","server.js"]
```

---

## Frontend Dockerfile

```dockerfile
FROM node:20 AS builder

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

FROM nginx:alpine

COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx","-g","daemon off;"]
```

---

## Commands

Start:

```
docker compose up --build
```

Stop:

```
docker compose down
```

Logs:

```
docker compose logs -f
```

---

## Architecture

Frontend → Backend → Database + Cache

React → Node → MongoDB + Redis
Flask → Postgres + Redis

---

## Production Concepts

- Multi-stage builds
- Distroless images
- Healthchecks
- Persistent volumes
- Custom networks

