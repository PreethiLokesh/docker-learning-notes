# Public and Private Registries

## Overview

Docker Images need a place where they can be stored, shared, downloaded, and versioned.

This storage location is called a Docker Registry.

A Registry is to Docker Images what GitHub is to Source Code.

```text
GitHub        -> Stores Source Code

Docker Hub    -> Stores Docker Images
```

---

# What is a Docker Registry?

A Docker Registry is a repository used to store and distribute Docker Images.

Developers can:

- Push Images
- Pull Images
- Share Images
- Version Images

---

# Why Do We Need Registries?

Suppose you create an application image:

```bash
docker build -t myapp:v1 .
```

This image is stored only on your laptop.

```text
Laptop
 |
 +-- myapp:v1
```

If another developer wants to run the same application, they need access to the image.

Solution:

Push the image to a Docker Registry.

```text
Developer
    |
Build Image
    |
    v
 Registry
    |
    v
Other Developers
```

---

# Public Registry

A Public Registry allows anyone to:

- View images
- Download images
- Use images

The images are publicly accessible.

---

# Docker Hub

Docker Hub is the most popular public registry.

Website:

```text
https://hub.docker.com
```

Docker Hub contains thousands of ready-made images.

Examples:

```text
nginx
ubuntu
node
redis
mongodb
mysql
postgres
python
```

---

# Pulling an Image

Download an image from Docker Hub:

```bash
docker pull nginx
```

Docker downloads:

```text
nginx:latest
```

to the local machine.

---

# Running Downloaded Images

```bash
docker run nginx
```

Docker:

```text
1. Pulls image (if missing)
2. Creates container
3. Starts container
```

---

# Registry Workflow

```text
Dockerfile
    |
    v
Build Image
    |
    v
Push Image
    |
    v
Registry
    |
    v
Pull Image
    |
    v
Run Container
```

---

# Popular Public Registries

## 1. Docker Hub

Most commonly used registry.

Examples:

```text
docker pull nginx
docker pull ubuntu
```

---

## 2. GitHub Container Registry

Provided by GitHub.

Used to store container images alongside source code.

---

## 3. Azure Container Registry (ACR)

Microsoft Azure container registry service.

Used by organizations running workloads on Azure.

---

## 4. Amazon Elastic Container Registry (ECR)

AWS-managed container registry.

Used in AWS environments.

---

## 5. Google Artifact Registry

Google Cloud registry service.

Commonly used in GCP projects.

---

# Private Registry

A Private Registry restricts access to authorized users.

Only users with permissions can:

- Push Images
- Pull Images
- View Images

---

# Why Use Private Registries?

Companies usually develop proprietary software.

Example:

```text
Internal Banking Application

Company ERP System

Internal Monitoring Tools
```

These applications must not be publicly accessible.

Therefore companies use Private Registries.

---

# Example

```text
Company Registry

backend:v1
frontend:v1
auth-service:v3
payment-service:v5
```

Only approved users can access these images.

---

# Public vs Private Registry

## Public Registry

Advantages:

- Easy sharing
- Open-source collaboration
- Free access to popular images

Examples:

```text
Docker Hub
GitHub Container Registry
```

---

## Private Registry

Advantages:

- Better security
- Access control
- Protects business applications
- Internal usage only

Examples:

```text
Azure Container Registry
Amazon ECR
Private Docker Registry
```

---

# Docker Image Naming Convention

Images generally follow:

```text
repository/image:tag
```

Format:

```text
<repository>/<image>:<tag>
```

---

# Examples

```text
nginx:latest

node:22

ubuntu:24.04
```

---

# Repository

Repository groups related image versions.

Example:

```text
node
```

contains:

```text
node:18
node:20
node:22
```

---

# Understanding Tags

A Tag represents an image version.

Example:

```text
node:22
```

Here:

```text
node = repository
22   = tag
```

---

# More Examples

```text
nginx:latest

ubuntu:24.04

postgres:17
```

---

# Why Tags Matter

Without tags:

```bash
docker pull node
```

Docker pulls:

```text
node:latest
```

But "latest" may change over time.

Explicit tags provide consistency.

Preferred:

```bash
docker pull node:22
```

---

# Push and Pull Operations

## Pull

Download image from Registry.

```bash
docker pull nginx
```

---

## Push

Upload image to Registry.

```bash
docker push username/myapp:v1
```

---

# Basic Developer Workflow

## Step 1

Create Dockerfile.

```text
Dockerfile
```

---

## Step 2

Build Image.

```bash
docker build -t myapp:v1 .
```

---

## Step 3

Push Image.

```bash
docker push username/myapp:v1
```

---

## Step 4

Another developer downloads.

```bash
docker pull username/myapp:v1
```

---

## Step 5

Run Container.

```bash
docker run username/myapp:v1
```

---

# Real-World Example

A company develops:

```text
Inventory Management Application
```

Build image:

```bash
docker build -t inventory:v1 .
```

Push to registry:

```bash
docker push company/inventory:v1
```

Production servers pull:

```bash
docker pull company/inventory:v1
```

Run:

```bash
docker run company/inventory:v1
```

Same image runs everywhere.

---

# Interview Questions

## What is a Docker Registry?

A Docker Registry is a storage and distribution system for Docker Images.

---

## What is Docker Hub?

Docker Hub is Docker's public registry used for storing and sharing container images.

---

## What is the difference between Public and Private Registries?

Public Registry:

- Accessible by everyone

Private Registry:

- Restricted access
- Requires authentication

---

## What is a Tag?

A Tag is a version identifier attached to a Docker Image.

Example:

```text
node:22
```

Tag:

```text
22
```

---

## What is the difference between Pull and Push?

Pull:

```text
Registry -> Local Machine
```

Push:

```text
Local Machine -> Registry
```

---

# Important Commands

Pull Image:

```bash
docker pull nginx
```

Build Image:

```bash
docker build -t myapp:v1 .
```

Push Image:

```bash
docker push username/myapp:v1
```

List Images:

```bash
docker images
```

---

# Quick Revision

```text
REGISTRY = Storage Location for Docker Images

Public Registry:
- Open Access
- Example: Docker Hub

Private Registry:
- Restricted Access
- Used by Companies

Popular Registries:
- Docker Hub
- Azure Container Registry
- Amazon ECR
- GitHub Container Registry
- Google Artifact Registry

Image Format:
repository/image:tag

Example:
nginx:latest
node:22
ubuntu:24.04

Pull:
Registry -> Local Machine

Push:
Local Machine -> Registry
```