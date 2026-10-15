# Image vs Container 

## Overview

One of the most important Docker concepts is understanding the difference between an **Image** and a **Container**.

A Docker Image is a blueprint or template.
A Docker Container is a running instance of that blueprint.

---

# What is a Docker Image?

A Docker Image is a read-only template that contains everything required to run an application.

An Image typically contains:

- Application Code
- Runtime Environment
- Libraries
- Dependencies
- Configuration Files

Think of an Image as a blueprint or recipe.

---

# What is a Docker Container?

A Docker Container is a running instance of a Docker Image.

When Docker runs an Image, it creates a Container.

A Container contains:

- Application Process
- Application Data
- Writable Layer

Think of a Container as the actual application running in memory.

---

# Simple Analogy

## Recipe vs Food

```text
Recipe = Image
Food = Container
```

A recipe contains instructions.

The recipe itself is not food.

You use the recipe to prepare food.

Similarly:

```text
Image -> Container
```

---

# Python Analogy

Consider a Python class:

```python
class Student:
    pass
```

The class is only a blueprint.

Create objects:

```python
s1 = Student()
s2 = Student()
```

Relationship:

```text
Class = Image
Object = Container
```

One class can create multiple objects.

One image can create multiple containers.

---

# Image Characteristics

## 1. Read-Only

Images cannot be modified while running.

They serve as immutable templates.

---

## 2. Reusable

The same image can be used repeatedly.

Example:

```text
nginx image
```

can create multiple containers.

---

## 3. Portable

Images can be:

- Shared
- Downloaded
- Uploaded
- Stored in Registries

---

## 4. Versioned

Images support tags.

Examples:

```text
nginx:latest
ubuntu:24.04
node:22
```

---

# Container Characteristics

## 1. Running Environment

Containers are active runtime instances of images.

---

## 2. Writable Layer

Containers can be modified.

Example:

```bash
touch file.txt
```

Changes occur inside the container.

---

## 3. Isolated

Containers run independently of each other.

---

## 4. Temporary by Default

If a container is deleted, its internal data may be lost unless Docker Volumes are used.

---

# Docker Workflow

```text
Dockerfile
     |
     v
Image
     |
 docker run
     |
     v
Container
```

---

# How an Image is Created

Developer writes:

```dockerfile
FROM node:22
COPY . .
RUN npm install
CMD ["npm", "start"]
```

Build image:

```bash
docker build -t myapp:v1 .
```

Result:

```text
myapp:v1
```

This is an image.

---

# How a Container is Created

Run the image:

```bash
docker run myapp:v1
```

Docker creates a container.

---

# Container Creation Process

```text
Step 1: Create Image

         myapp:v1

Step 2: Run Image

 docker run myapp:v1

Step 3: Container Starts

 Running Container
```

---

# One Image → Multiple Containers

Example:

```bash
docker run nginx
docker run nginx
docker run nginx
```

Result:

```text
nginx image
     |
     +--> Container 1
     |
     +--> Container 2
     |
     +--> Container 3
```

A single image can create many containers.

---

# Image and Container Lifecycle

## Pull an Image

```bash
docker pull nginx
```

Image stored locally.

---

## Run Container

```bash
docker run nginx
```

Container created and started.

---

## Stop Container

```bash
docker stop container_name
```

Container remains on system.

---

## Start Container Again

```bash
docker start container_name
```

Existing container starts again.

---

## Remove Container

```bash
docker rm container_name
```

Container deleted.

Image still exists.

---

# Important Concept

Deleting a container does NOT delete the image.

Example:

```text
Image ✅
Container ❌
```

You can create a new container from the same image.

---

# Nginx Example

Pull Image:

```bash
docker pull nginx
```

Image:

```text
nginx image
```

Run Container:

```bash
docker run nginx
```

Now:

```text
nginx image
      |
      v
nginx container
```

---

# Image Storage vs Container Storage

## Image Layer

```text
Read-Only
```

Shared between containers.

---

## Container Layer

```text
Writable
```

Unique to each container.

---

# Difference Between Image and Container

## Docker Image

- Blueprint
- Template
- Read-only
- Stored on Disk
- Not Running
- Creates Containers

## Docker Container

- Running Instance
- Writable
- Active Process
- Uses Memory and CPU
- Created from Image

---

# Interview Questions

## What is a Docker Image?

A Docker Image is a read-only template that contains the application code, dependencies, libraries, runtime, and configuration needed to run a container.

---

## What is a Docker Container?

A Docker Container is a running instance of a Docker Image.

---

## Can one image create multiple containers?

Yes.

```text
One Image → Many Containers
```

---

## Is an Image running?

No.

An Image is only a template.

---

## What command creates a container?

```bash
docker run
```

---

## When a container is deleted, does the image get deleted?

No.

The image remains available.

---

# Quick Revision

```text
IMAGE = Blueprint / Recipe / Template

CONTAINER = Running Instance of an Image

Dockerfile
    ↓
Image
    ↓
docker run
    ↓
Container

Image:
- Read-only
- Stored on Disk
- Not Running

Container:
- Running Application
- Writable
- Uses CPU and Memory

One Image → Many Containers

Deleting Container ≠ Deleting Image
```
