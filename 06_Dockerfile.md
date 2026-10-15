# Create Your Own Image (Dockerfile) 

## Overview

Docker allows developers to create their own custom images instead of relying only on public images such as:

```text
nginx
ubuntu
node
mysql
```

To create a custom image, Docker uses a file called a **Dockerfile**.

A Dockerfile contains instructions that Docker follows to build an image.

---

# Why Do We Need Dockerfile?

Suppose you have a Node.js application.

Without Docker:

```text
Install Node.js
Install Dependencies
Copy Application Code
Run Application
```

Every developer or server must repeat these steps.

Docker solves this problem by packaging everything into a Docker Image.

The build instructions are stored in:

```text
Dockerfile
```

---

# What is a Dockerfile?

A Dockerfile is a text file containing instructions used to build a Docker Image.

Example:

```dockerfile
FROM ubuntu

CMD ["echo", "Hello Docker"]
```

Docker reads these instructions and creates an image.

---

# Docker Workflow

```text
Dockerfile
     ↓
docker build
     ↓
Docker Image
     ↓
docker run
     ↓
Container
```

---

# First Dockerfile Example

Create a file named:

```text
Dockerfile
```

Content:

```dockerfile
FROM ubuntu

CMD ["echo", "Hello Docker"]
```

Build Image:

```bash
docker build -t myimage .
```

Run Container:

```bash
docker run myimage
```

Output:

```text
Hello Docker
```

---

# Dockerfile Instructions

The most common Dockerfile instructions are:

```text
FROM
RUN
WORKDIR
COPY
CMD
```

These are enough for most beginner projects.

---

# FROM Instruction

## Purpose

Specifies the base image.

Every Dockerfile starts with a FROM instruction.

Example:

```dockerfile
FROM ubuntu
```

Example:

```dockerfile
FROM node:22
```

Example:

```dockerfile
FROM python:3.12
```

Think of FROM as:

```text
Starting Point
```

---

# RUN Instruction

## Purpose

Executes commands while building the image.

Example:

```dockerfile
RUN apt-get update
```

Install Software:

```dockerfile
RUN apt-get install -y curl
```

Multiple Commands:

```dockerfile
RUN apt-get update && apt-get install -y curl
```

RUN executes during:

```text
docker build
```

NOT during:

```text
docker run
```

---

# WORKDIR Instruction

## Purpose

Sets the working directory inside the image.

Example:

```dockerfile
WORKDIR /app
```

After this:

```dockerfile
COPY . .
```

copies files into:

```text
/app
```

instead of:

```text
/
```

---

# COPY Instruction

## Purpose

Copies files from host machine to image.

Example:

```dockerfile
COPY . .
```

Meaning:

```text
Current Directory
       ↓
Container Working Directory
```

---

Copy Specific File:

```dockerfile
COPY app.js .
```

Copies:

```text
app.js
```

into the image.

---

Copy Folder:

```dockerfile
COPY src ./src
```

Copies:

```text
Host src Folder
       ↓
Image src Folder
```

---

# CMD Instruction

## Purpose

Defines the default command executed when a container starts.

Example:

```dockerfile
CMD ["node", "app.js"]
```

When container starts:

```bash
node app.js
```

runs automatically.

---

Example:

```dockerfile
CMD ["python", "app.py"]
```

Container starts:

```bash
python app.py
```

---

# Difference Between RUN and CMD

## RUN

Executed during image build.

Example:

```dockerfile
RUN npm install
```

Executed when:

```bash
docker build
```

runs.

---

## CMD

Executed when container starts.

Example:

```dockerfile
CMD ["npm", "start"]
```

Executed when:

```bash
docker run
```

runs.

---

# Simple Node.js Example

## app.js

```javascript
console.log("Hello from Docker");
```

---

## Dockerfile

```dockerfile
FROM node:22

WORKDIR /app

COPY . .

CMD ["node", "app.js"]
```

---

# Build the Image

Command:

```bash
docker build -t nodeapp .
```

Explanation:

```text
docker build     -> Build Image

-t               -> Tag

nodeapp          -> Image Name

.                -> Current Directory
```

---

# Run the Image

```bash
docker run nodeapp
```

Output:

```text
Hello from Docker
```

---

# The Meaning of Dot (.)

Example:

```bash
docker build -t myapp .
```

The dot means:

```text
Current Directory
```

Docker searches the current directory for:

```text
Dockerfile
```

---

# View Created Images

```bash
docker images
```

Example:

```text
REPOSITORY    TAG

nodeapp       latest

nginx         latest

ubuntu        latest
```

---

# Image Versioning During Build

Build Version 1:

```bash
docker build -t myapp:v1 .
```

Build Version 2:

```bash
docker build -t myapp:v2 .
```

Images Created:

```text
myapp:v1

myapp:v2
```

---

# Understanding Docker Layers

Each Dockerfile instruction creates a layer.

Example:

```dockerfile
FROM ubuntu

RUN apt-get update

RUN apt-get install -y curl

COPY . .
```

Docker Layers:

```text
Layer 1 → Ubuntu

Layer 2 → apt-get update

Layer 3 → Install curl

Layer 4 → Copy files
```

---

# Why Layers are Important

Docker caches layers.

If a layer does not change:

```text
Docker reuses it
```

As a result:

```text
Faster Build Times
```

---

# Typical Node.js Dockerfile

```dockerfile
FROM node:22

WORKDIR /app

COPY package*.json ./

*UN npm install

COPY . .

CMD ["npm", "start"]
```

---
# Build and Run Node.js Application

Build:

```bash
docker build -t nodeapp:v1 .
```

Run:

```bash
docker run -p 3000:3000 nodeapp:v1
```

Port Mapping:

```text
Host Port 3000
         ↓
Container Port 3000
```

---

# Dockerfile Best Practices

## 1. Use Specific Image Tags

Avoid:

```dockerfile
FROM node:latest
```

Prefer:

```dockerfile
FROM node:22
```

---

## 2. Set a Working Directory

Use:

```dockerfile
WORKDIR /app
```

instead of working in:

```text
/
```

---

## 3. Minimize Layers

Prefer:

```dockerfile
RUN apt-get update && apt-get install -y curl
```

instead of multiple RUN statements.

---

## 4. Keep Images Small

Install only necessary packages.

Smaller images:

```text
Build Faster
Transfer Faster
Start Faster
```

---

# Common Dockerfile Instructions Summary

## FROM

Base Image

Example:

```dockerfile
FROM ubuntu
```

---

## RUN

Execute build-time commands.

Example:

```dockerfile
RUN apt-get update
```

---

## WORKDIR

Set working directory.

Example:

```dockerfile
WORKDIR /app
```

---

## COPY

Copy files into image.

Example:

```dockerfile
COPY . .
```

---

## CMD

Default startup command.

Example:

```dockerfile
CMD ["node", "app.js"]
```

---

# Interview Questions

## What is a Dockerfile?

A Dockerfile is a text file containing instructions used to build a Docker Image.

---

## What does FROM do?

Specifies the base image used for building the new image.

Example:

```dockerfile
FROM ubuntu
```

---

## What does RUN do?

Executes commands during image creation.

Example:

```dockerfile
RUN apt-get update
```

---

## What does COPY do?

Copies files from the host machine into the image.

Example:

```dockerfile
COPY . .
```

---

## What does WORKDIR do?

Sets the working directory inside the container.

Example:

```dockerfile
WORKDIR /app
```

---

## What does CMD do?

Defines the default command that runs when the container starts.

Example:

```dockerfile
CMD ["node", "app.js"]
```

---

## Difference Between RUN and CMD?

### RUN

```text
Executed while building the image
```

### CMD

```text
Executed while starting the container
```

---

# Quick Revision

```text
Dockerfile
      ↓
docker build
      ↓
Image
      ↓
docker run
      ↓
Container

Most Important Instructions:

FROM
RUN
WORKDIR
COPY
CMD

FROM
→ Base Image

RUN
→ Execute Build Commands

WORKDIR
→ Set Working Directory

COPY
→ Copy Files

CMD
→ Startup Command

Build:
docker build -t myapp:v1 .

Run:
docker run myapp:v1

RUN
→ Build Time

CMD
→ Container Start Time
```
```text

FROM     → Start with which image?
RUN      → What to install?
WORKDIR  → Where to work?
COPY     → Which files to copy?
CMD      → What to run when container starts?

```