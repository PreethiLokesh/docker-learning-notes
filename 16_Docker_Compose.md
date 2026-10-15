# Docker Compose 

## Overview

Docker Compose is a tool used to define and run multi-container Docker applications using a YAML configuration file.

Instead of starting containers one by one using multiple `docker run` commands, Docker Compose allows you to manage the entire application from a single file.

---

# Why Do We Need Docker Compose?

Without Compose:

```bash
docker network create app-net

docker run -d \
--name mysql \
--network app-net \
-v mysql-data:/var/lib/mysql \
mysql

docker run -d \
--name backend \
--network app-net \
-p 3000:3000 \
backend-image
```

As applications grow, managing everything manually becomes difficult.

---

# Solution: Docker Compose

With Compose:

```bash
docker compose up
```

Docker automatically creates:

```text
Containers

Networks

Volumes

Environment Variables
```

---

# What is Docker Compose?

Docker Compose is a tool for managing multi-container applications using a YAML file.

Common filenames:

```text
compose.yaml
```

or

```text
docker-compose.yml
```

---

# Basic Compose File

Example:

```yaml
services:
  nginx:
    image: nginx
```

Run:

```bash
docker compose up
```

Docker creates:

```text
nginx container
```

---

# Understanding the Structure

```yaml
services:
```

All containers are defined under the `services` section.

Example:

```yaml
services:
  frontend:
    image: nginx

  backend:
    image: node:22
```

---

# Service Name vs Image Name

Example:

```yaml
services:
  nginx:
    image: nginx
```

First `nginx`:

```text
Service Name
```

Second `nginx`:

```text
Docker Image
```

---

Example:

```yaml
services:
  webserver:
    image: nginx
```

Meaning:

```text
Service Name → webserver

Image → nginx
```

---

# Multiple Services Example

```yaml
services:

  backend:
    image: node:22

  database:
    image: mysql
```

Compose starts:

```text
Backend Container

Database Container
```

---

# Running Compose

Start application:

```bash
docker compose up
```

---

Run in background:

```bash
docker compose up -d
```

Meaning:

```text
Detached Mode
```

---

# Port Mapping

Docker Run Version:

```bash
docker run -p 3000:3000 nginx
```

Compose Version:

```yaml
services:

  backend:
    image: node:22

    ports:
      - "3000:3000"
```

Meaning:

```text
Host Port 3000
         ↓
Container Port 3000
```

---

# Environment Variables

Docker Run Version:

```bash
docker run \
-e DB_USER=admin \
-e DB_PASSWORD=secret
```

Compose Version:

```yaml
services:

  backend:

    environment:
      DB_USER: admin
      DB_PASSWORD: secret
```

---

# Using .env Files

Create:

```text
.env
```

Contents:

```env
DB_USER=admin
DB_PASSWORD=secret
```

Compose:

```yaml
environment:
  DB_USER: ${DB_USER}
  DB_PASSWORD: ${DB_PASSWORD}
```

Docker automatically reads values from `.env`.

---

# Volumes in Compose

Docker Run Version:

```bash
docker run -v mysql-data:/var/lib/mysql mysql
```

Compose Version:

```yaml
services:

  mysql:
    image: mysql

    volumes:
      - mysql-data:/var/lib/mysql

volumes:
  mysql-data:
```

---

# Understanding Volume Mapping

```text
mysql-data
      ↓
Docker Volume

/var/lib/mysql
      ↓
Folder Inside Container
```

Data survives container deletion.

---

# Networks in Compose

Docker Run Version:

```bash
docker network create app-net
```

Compose Version:

```yaml
networks:
  app-net:
```

Attach Services:

```yaml
services:

  backend:
    networks:
      - app-net

  mysql:
    networks:
      - app-net
```

Both containers join:

```text
app-net
```

---

# Complete Compose Example

```yaml
services:

  backend:
    image: node:22

    ports:
      - "3000:3000"

    environment:
      DB_HOST: mysql

    networks:
      - app-net

  mysql:
    image: mysql

    environment:
      MYSQL_ROOT_PASSWORD: secret

    volumes:
      - mysql-data:/var/lib/mysql

    networks:
      - app-net

volumes:
  mysql-data:

networks:
  app-net:
```

---

# What Happens Internally?

Run:

```bash
docker compose up
```

Docker automatically creates:

```text
backend container

mysql container

mysql-data volume

app-net network
```

---

# Container Communication

Compose automatically provides DNS.

Example:

```yaml
services:

  backend:

  mysql:
```

Backend can connect using:

```text
mysql
```

Example:

```env
DB_HOST=mysql
```

No IP address required.

---

# Useful Compose Commands

## Start Application

```bash
docker compose up
```

---

## Start in Background

```bash
docker compose up -d
```

---

## Stop Application

```bash
docker compose down
```

---

## View Running Services

```bash
docker compose ps
```

---

## View Logs

```bash
docker compose logs
```

---

## Follow Logs

```bash
docker compose logs -f
```

---

## Rebuild Containers

```bash
docker compose up --build
```

Used when code changes.

---

# Typical Project Structure

```text
project/
|
├── compose.yaml
├── .env
├── Dockerfile
├── app.js
└── package.json
```

---

# Why Docker Compose is Important

Compose combines:

```text
Containers

Volumes

Networks

Environment Variables
```

into a single configuration file.

---

# Docker Compose vs Docker Run

## Docker Run

```bash
docker run nginx
```

Best For:

```text
Single Container
```

---

## Docker Compose

```bash
docker compose up
```

Best For:

```text
Multi-Container Applications
```

---

# Relationship With Kubernetes

Docker Compose:

```yaml
services:
  backend:
  mysql:
```

Kubernetes:

```yaml
apiVersion:
kind:
metadata:
spec:
```

Both use YAML files.

Learning Compose makes Kubernetes easier.

---

# Interview Questions

## What is Docker Compose?

Docker Compose is a tool used to define and manage multi-container Docker applications using a YAML file.

---

## Which File Does Docker Compose Use?

```text
compose.yaml
```

or

```text
docker-compose.yml
```

---

## What Does docker compose up Do?

Starts all services defined in the Compose file.

---

## What Does docker compose down Do?

Stops and removes containers created by Docker Compose.

---

## Why Use Docker Compose?

To manage:

```text
Multiple Containers

Networks

Volumes

Environment Variables
```

through a single configuration file.

---

## Difference Between docker run and docker compose?

docker run:

```text
Manages One Container
```

docker compose:

```text
Manages Entire Application
```

---

# Quick Revision

```text
Docker Compose
        ↓
Multi-Container Management

File:

compose.yaml

Commands:

docker compose up

docker compose up -d

docker compose down

docker compose ps

docker compose logs

docker compose up --build

Compose Manages:

Containers

Volumes

Networks

Environment Variables
```

---

# Memory Tricks

## Service Name vs Image

```yaml
services:
  backend:
    image: node:22
```

```text
backend
   ↓
Service Name

node:22
   ↓
Docker Image
```

---

## Compose Purpose

```text
docker run
      ↓
One Container

docker compose
      ↓
Entire Application
```

---

# Final Summary

Docker Compose allows you to define containers, networks, volumes, and environment variables in a single YAML file and start the entire application with:

```bash
docker compose up
```

instead of managing everything manually using multiple `docker run` commands.