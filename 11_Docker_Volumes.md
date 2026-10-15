# Docker Volumes 

## Overview

Containers are temporary by nature.

Any data stored inside a container can be lost when the container is removed.

Docker Volumes solve this problem by providing persistent storage.

---

# The Problem Without Volumes

Suppose a container stores data:

```text
Container
│
└── file.txt
```

Delete the container:

```bash
docker rm mycontainer
```

Result:

```text
Container Deleted

Data Deleted
```

All container data is lost.

---

# Why Is This a Problem?

Applications such as:

```text
MySQL
PostgreSQL
MongoDB
Redis
User Upload Systems
Log Storage Systems
```

must preserve data.

If data disappears whenever a container is deleted:

```text
Database Records Lost

Customer Data Lost

Logs Lost
```

This is unacceptable.

---

# Solution: Docker Volumes

Volumes store data outside the container.

Architecture:

```text
Container
     |
     v
Docker Volume
     |
     v
Persistent Storage
```

Now:

```text
Container Deleted ❌

Volume Remains ✅
```

Data survives.

---

# Simple Analogy

Without Volume:

```text
Notebook kept inside a rented room

Room deleted
Notebook lost
```

With Volume:

```text
Notebook stored in a locker

Room deleted
Locker remains
```

Volume = Locker

Container = Room

---

# What Is a Docker Volume?

A Docker Volume is a storage area managed by Docker.

Volumes:

```text
Store Data

Persist Data

Can Be Shared Between Containers
```

---

# Volume Architecture

Without Volume:

```text
Container
    |
Application Data
```

Delete Container:

```text
Everything Lost
```

---

With Volume:

```text
Container
     |
     v
Volume
     |
     v
Persistent Data
```

Delete Container:

```text
Container Deleted

Volume Still Exists
```

---

# Types of Docker Storage

Docker provides three storage methods:

```text
1. Named Volumes
2. Anonymous Volumes
3. Bind Mounts
```

---

# 1. Named Volumes (Most Important)

Named Volumes have a user-defined name.

Create a volume:

```bash
docker volume create mydata
```

Check volumes:

```bash
docker volume ls
```

Output:

```text
DRIVER    VOLUME NAME

local     mydata
```

---

# Using a Named Volume

Command:

```bash
docker run -v mydata:/app/data nginx
```

Structure:

```text
mydata:/app/data

mydata
   ↓
Docker Volume

/app/data
   ↓
Folder Inside Container
```

---

# Understanding the Command

Docker creates or uses a volume:

```text
mydata
```

and mounts it inside container at:

```text
/app/data
```

Visual:

```text
Volume: mydata
        ^
        |
        |
Container: /app/data
```

---

# Data Storage Example

Inside container:

```bash
echo "Hello Docker" > /app/data/file.txt
```

File is stored in:

```text
Volume mydata
```

NOT inside the container layer.

---

# What Happens If Container Is Deleted?

Delete container:

```bash
docker rm -f mycontainer
```

Result:

```text
Container Deleted

Volume Exists
```

Data remains:

```text
mydata
│
└── file.txt
```

---

# Create a New Container

Run again:

```bash
docker run -v mydata:/app/data nginx
```

New container connects to:

```text
Volume mydata
```

File still exists:

```bash
cat /app/data/file.txt
```

Output:

```text
Hello Docker
```

---

# Why Use /app/data?

Nothing special about this path.

Examples:

```bash
docker run -v mydata:/data nginx
```

```bash
docker run -v mydata:/storage nginx
```

```bash
docker run -v mydata:/app/data nginx
```

All are valid.

General Pattern:

```bash
docker run -v <volume-name>:<container-folder> <image>
```

---

# Managing Volumes

List Volumes:

```bash
docker volume ls
```

---

Inspect Volume:

```bash
docker volume inspect mydata
```

Shows:

```text
Mount Point

Driver

Configuration
```

---

Delete Volume:

```bash
docker volume rm mydata
```

---

# 2. Anonymous Volumes

Docker automatically generates volume names.

Example:

```bash
docker run -v /app/data nginx
```

Volume Example:

```text
7d91ab3d2f4f...
```

Problem:

```text
Hard To Identify

Hard To Manage
```

Therefore less commonly used.

---

# 3. Bind Mounts

Used heavily during development.

Example:

```bash
docker run -v $(pwd):/app nginx
```

Meaning:

```text
Current Host Folder
          ↓
Container Folder
```

---

# Bind Mount Example

Host Machine:

```text
project/
├── app.js
├── package.json
└── Dockerfile
```

Command:

```bash
docker run -v $(pwd):/app node
```

Inside Container:

```text
/app
├── app.js
├── package.json
└── Dockerfile
```

Same files are visible.

---

# Why Bind Mounts Are Useful

Without Bind Mount:

```text
Change Code
      ↓
Rebuild Image
      ↓
Restart Container
```

With Bind Mount:

```text
Change Code
      ↓
Immediately Visible
      ↓
No Rebuild Needed
```

Perfect for development.

---

# Volume Lifecycle

Step 1:

Create Volume

```bash
docker volume create mydata
```

---

Step 2:

Attach Volume

```bash
docker run -v mydata:/data nginx
```

---

Step 3:

Delete Container

```bash
docker rm mycontainer
```

Volume remains.

---

Step 4:

Verify Volume

```bash
docker volume ls
```

---

Step 5:

Delete Volume

```bash
docker volume rm mydata
```

---

# Real-World Examples

## MySQL

```bash
docker run \
-v mysql-data:/var/lib/mysql \
mysql
```

Database remains even if container is removed.

---

## PostgreSQL

```bash
docker run \
-v pg-data:/var/lib/postgresql/data \
postgres
```

---

## MongoDB

```bash
docker run \
-v mongo-data:/data/db \
mongo
```

---

# Named Volume vs Bind Mount

## Named Volume

Example:

```bash
docker run -v mydata:/data nginx
```

Characteristics:

```text
Managed by Docker

Persistent

Easy Backup

Production Friendly
```

Best For:

```text
Databases

Production Storage
```

---

## Bind Mount

Example:

```bash
docker run -v $(pwd):/app nginx
```

Characteristics:

```text
Managed by Host OS

Uses Existing Folders

Reflects Instant Changes
```

Best For:

```text
Development

Source Code
```

---

# Important Commands

Create Volume:

```bash
docker volume create mydata
```

---

List Volumes:

```bash
docker volume ls
```

---

Inspect Volume:

```bash
docker volume inspect mydata
```

---

Use Volume:

```bash
docker run -v mydata:/data nginx
```

---

Delete Volume:

```bash
docker volume rm mydata
```

---

# Interview Questions

## Why Are Docker Volumes Needed?

Volumes provide persistent storage that survives container deletion.

---

## What Happens When a Container Is Deleted?

```text
Container Data → Lost

Volume Data → Preserved
```

---

## What Are the Types of Docker Storage?

```text
Named Volumes

Anonymous Volumes

Bind Mounts
```

---

## Difference Between Volume and Bind Mount?

Volume:

```text
Managed by Docker
```

Bind Mount:

```text
Managed by Host Operating System
```

---

## Which Volume Type Is Commonly Used in Production?

```text
Named Volumes
```

---

# Quick Revision

```text
Problem:

Container Deleted
        ↓
Data Lost

Solution:

Docker Volume
        ↓
Persistent Storage

Types:

1. Named Volume
2. Anonymous Volume
3. Bind Mount

Named Volume:

mydata:/data

Bind Mount:

$(pwd):/app

Commands:

docker volume create mydata

docker volume ls

docker volume inspect mydata

docker volume rm mydata
```

---

# Memory Trick

```text
Container = Application

Volume = Storage

Delete Container
       ↓
Application Gone

Volume Remains
       ↓
Data Survives
```

## Most Important Line to Remember

```bash
docker run -v mydata:/app/data nginx
```

Meaning:

```text
Create/Use a Docker Volume called mydata
and mount it inside the container at
/app/data so data survives container deletion.
```


## Most Common Interview Question
What does this command mean?

```
docker run -v mydata:/app/data nginx
```

```
Answer:

It creates or uses a Docker volume named mydata and mounts it to /app/data inside the container. Any data written to /app/data is stored in the volume and survives container deletion.
```