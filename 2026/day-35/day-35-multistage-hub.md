# Day 35 -- Multi-Stage Builds & Docker Hub

## Dockerfiles

### Single Stage

    FROM node:latest

    WORKDIR /app

    COPY package*.json .

    RUN npm install

    COPY . .

    EXPOSE 3000

    CMD ["node","app.js"]

------------------------------------------------------------------------

### Multi Stage

    FROM node:latest AS builder

    WORKDIR /app

    COPY package*.json .

    RUN npm install

    COPY . .

    FROM node:20-alpine

    WORKDIR /app

    COPY --from=builder /app .

    EXPOSE 3000

    CMD ["node","app.js"]

------------------------------------------------------------------------

### Production Dockerfile

    FROM node:20-alpine AS builder

    WORKDIR /app

    COPY package*.json .

    RUN npm install

    COPY . .

    FROM node:20-alpine

    WORKDIR /app

    RUN addgroup -S nodegroup && adduser -S nodeuser -G nodegroup

    COPY --from=builder /app .

    USER nodeuser

    EXPOSE 3000

    CMD ["node","app.js"]

------------------------------------------------------------------------

## Docker Hub Repo

https://hub.docker.com/repository/docker/nitinkumarpal/node-backend/general
