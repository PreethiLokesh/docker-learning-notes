# Docker Networks 

## Overview

Docker Networks allow containers to communicate with each other.

Example:

```text
Frontend Container
       ↓
Backend Container
       ↓
Database Container
```

Without a network, containers cannot communicate properly.

---

# Why Do We Need Docker Networks?

Modern applications are usually split into multiple containers.

Example:

```text
React Frontend

Node.js Backend

MySQL Database
```

Question:

```text
How does the backend talk to MySQL?
```

Answer:

```text
Docker Network
```

---

# What is a Docker Network?

A Docker Network is a communication layer that allows containers to talk to each other.

Analogy:

```text
Containers = Houses

Network = Roads
```

Without roads:

```text
House A ❌ House B
```

No communication.

With roads:

```text
House A ↔ House B
```

Communication becomes possible.

---

# Docker Network Types

Docker provides:

```text
1. Bridge Network
2. Host Network
3. None Network
4. Custom Bridge Network
```

---

# 1. Bridge Network

Default Docker network.

View networks:

```bash
docker network ls
```

Example:

```text
NETWORK NAME

bridge

host

none
```

Every container automatically joins the bridge network unless specified otherwise.

Example:

```bash
docker run nginx
```

Docker automatically attaches:

```text
nginx
  ↓
bridge network
```

---

# Default Bridge Network

```text
bridge
   |
   └── nginx container
```

Docker does this automatically.

---

# Limitation of Bridge Network

Fine for:

```text
Single Containers
```

Not ideal for:

```text
Multi-container Applications
```

For production-style applications, custom networks are preferred.

---

# 2. Custom Bridge Network

Most commonly used network type.

Create:

```bash
docker network create mynet
```

List:

```bash
docker network ls
```

Output:

```text
bridge
host
none
mynet
```

---

# Connect Container to Custom Network

Example:

```bash
docker run -d \
--name frontend \
--network mynet \
nginx
```

```bash
docker run -d \
--name backend \
--network mynet \
nginx
```

Both containers now belong to:

```text
mynet
```

---

# Container Communication

Inside frontend:

```bash
ping backend
```

Works because Docker provides DNS.

Containers can communicate using names.

---

# Docker DNS

Docker automatically resolves:

```text
frontend

backend

mysql
```

to container IP addresses.

This means:

```text
backend → mysql
```

instead of:

```text
backend → 172.x.x.x
```

---

# Real Example

Create network:

```bash
docker network create app-net
```

Run MySQL:

```bash
docker run -d \
--name mysql \
--network app-net \
mysql
```

Run Backend:

```bash
docker run -d \
--name backend \
--network app-net \
nodeapp
```

Backend can connect using:

```text
mysql:3306
```

instead of IP addresses.

---

# Visualization

```text
           app-net
              |
      -----------------
      |               |
      v               v

   backend         mysql
```

---

# 3. Host Network

Container uses host machine networking directly.

Example:

```bash
docker run --network host nginx
```

Architecture:

```text
Container
      ↓
Host Network Stack
```

Benefits:

```text
Better Performance
No Network Translation
```

Drawbacks:

```text
Less Isolation
```

---

# 4. None Network

No networking at all.

Example:

```bash
docker run --network none nginx
```

Result:

```text
No Internet

No Container Communication
```

Useful for:

```text
Security

Testing
```

---

# Important Docker Network Commands

List Networks:

```bash
docker network ls
```

Create Network:

```bash
docker network create mynet
```

Inspect Network:

```bash
docker network inspect mynet
```

Remove Network:

```bash
docker network rm mynet
```

---

# Inspect Network

Command:

```bash
docker network inspect mynet
```

Shows:

```text
Connected Containers

Gateway

Subnet

Configuration
```

---

# Networks vs Volumes

Volume Purpose:

```text
Store Data
```

Example:

```bash
docker run -v mydata:/data mysql
```

---

Network Purpose:

```text
Connect Containers
```

Example:

```bash
docker run --network mynet nginx
```

---

# Real Architecture Example

```text
Frontend
      ↓
Backend
      ↓
MySQL
```

Docker Version:

```text
Frontend Container
         |
         v
      app-net
         |
         v
Backend Container
         |
         v
      app-net
         |
         v
MySQL Container
```

All communicate through the same network.

---

# Interview Questions

## Why are Docker Networks needed?

Docker Networks allow containers to communicate with each other.

---

## What is the default Docker Network?

```text
Bridge Network
```

---

## Which network type is most commonly used?

```text
Custom Bridge Network
```

---

## Can containers communicate using names?

Yes.

Docker provides built-in DNS.

Examples:

```text
mysql

backend

frontend
```

---

## Difference Between Volume and Network?

Volume:

```text
Stores Data
```

Network:

```text
Connects Containers
```

---

# Quick Revision

```text
Docker Networks
       ↓
Container Communication

Types:

1. Bridge
2. Custom Bridge
3. Host
4. None

Commands:

docker network ls

docker network create mynet

docker network inspect mynet

docker network rm mynet

Most Used:

Custom Bridge Network

Benefit:

Container-to-Container Communication

Docker DNS:

backend → mysql

frontend → backend
```

---

# Memory Trick

```text
Volume
    ↓
Stores Data

Network
    ↓
Connects Containers
```