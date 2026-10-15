# Docker Fundamentals

## What is Docker?

Docker is a **containerization platform** that packages an application along with all its dependencies into containers, allowing it to run consistently across different environments.

> **Build once, run anywhere.**

---

# Why Docker?

Traditional software deployment often leads to situations where an application works on one machine but fails on another due to differences in operating systems, installed libraries, or software versions.

Docker eliminates these problems by packaging everything the application needs into a container.

## Docker Solves

- "Works on my machine" problem
- Dependency conflicts
- Deployment issues
- Environment inconsistencies

---

# What Does Docker Package?

A Docker container includes:

- Application code
- Dependencies
- Libraries
- Runtime environment

Because everything required is packaged together, the application behaves the same way regardless of where it is executed.

---

# Key Docker Concepts

## Docker

Docker is the platform used to create, manage, and run containers.

**Examples:**

- Docker Engine
- Docker Desktop

---

## Image

A Docker **Image** is a read-only blueprint or template used to create containers.

Think of an image as a recipe.

**Examples:**

```text
nginx
ubuntu
node:22
mysql
```

---

## Container

A Docker **Container** is a running instance of a Docker image.

Think of a container as the actual dish prepared from a recipe.

### Example

```text
Image: nginx

docker run nginx

Container: Running Nginx Web Server
```

---

# Relationship Between Image and Container

```text
Docker Image
      │
      ▼
 docker run
      │
      ▼
Docker Container
```

One image can create multiple containers.

```text
           nginx Image
                │
      ┌─────────┼─────────┐
      │         │         │
      ▼         ▼         ▼
Container1 Container2 Container3
```

---

# Benefits of Docker

## 1. Consistent Environments

Applications behave the same across:

- Development
- Testing
- Staging
- Production

---

## 2. Fast Deployment

Containers start within seconds because they share the host operating system kernel.

---

## 3. Isolation

Each container operates independently.

Problems inside one container usually do not affect other containers.

---

## 4. Scalability

Multiple container instances can be created easily to handle increased traffic.

### Example

```text
nginx-container-1
nginx-container-2
nginx-container-3
```

---

## 5. Portability

Containers can run on different platforms without modification.

### Supported Environments

- Local Machine
- Virtual Machines
- AWS
- Microsoft Azure
- Google Cloud Platform (GCP)
- Kubernetes Clusters

---

# Real-World Example

Suppose you build a Node.js application.

Without Docker:

```text
Install Node.js
Install Dependencies
Configure Environment
Fix Version Conflicts
Run Application
```

With Docker:

```text
docker build
docker run
```

The application runs the same on every machine because the entire environment is packaged inside the container.

---

# Quick Summary

```text
Docker     = Container Platform
Image      = Blueprint / Template
Container  = Running Instance of an Image
Runtime    = Environment Required to Run an Application
Dependency = External Software Required by an Application
```

---

# Interview Definition

Docker is a containerization platform that packages applications, dependencies, libraries, and runtime environments into lightweight containers, enabling consistent, portable, isolated, and scalable application deployment across different environments.
``