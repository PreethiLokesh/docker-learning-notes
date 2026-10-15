# Dockerfile Doubts Notes

## Understanding What to Write in FROM, WORKDIR, COPY, RUN, and CMD

---

# Core Idea

A Dockerfile is simply a translation of the manual steps required to run an application on a fresh machine.

Manual Process:

```text
Install Runtime
Create Project Folder
Copy Project Files
Install Dependencies
Start Application
```

Dockerfile Equivalent:

```text
FROM
WORKDIR
COPY
RUN
CMD
```

---

# FROM

## Question

```text
What runtime or platform does my application need?
```

Examples:

Node.js:

```dockerfile
FROM node:22
```

Python:

```dockerfile
FROM python:3.12
```

Java:

```dockerfile
FROM openjdk:21
```

Ubuntu:

```dockerfile
FROM ubuntu
```

Rule:

```text
FROM = Runtime required by the application
```

---

# WORKDIR

## Question

```text
Where should my application live inside the container?
```

Example:

```dockerfile
WORKDIR /app
```

Equivalent Manual Commands:

```bash
mkdir /app
cd /app
```

Common Choices:

```dockerfile
WORKDIR /app
```

```dockerfile
WORKDIR /usr/src/app
```

Rule:

```text
WORKDIR = Application folder inside the container
```

---

# COPY

## Question

```text
Which files does my application need?
```

Copy Everything:

```dockerfile
COPY . .
```

Copy One File:

```dockerfile
COPY app.js .
```

Copy Directory:

```dockerfile
COPY src ./src
```

Rule:

```text
COPY = Bring files from your computer into the image
```

---

# RUN

## Question

```text
What commands would I run to prepare the machine?
```

Examples:

```dockerfile
RUN npm install
```

```dockerfile
RUN pip install -r requirements.txt
```

```dockerfile
RUN apt-get update
```

Rule:

```text
RUN = Installation/setup commands executed during image build
```

---

# CMD

## Question

```text
How do I start the application?
```

Node.js:

```dockerfile
CMD ["node", "app.js"]
```

Python:

```dockerfile
CMD ["python", "app.py"]
```

React:

```dockerfile
CMD ["npm", "start"]
```

Rule:

```text
CMD = Startup command executed when the container starts
```

---

# Complete Example

Manual Steps:

```bash
Install Node.js

cd project

npm install

node app.js
```

Dockerfile:

```dockerfile
FROM node:22

WORKDIR /app

COPY . .

RUN npm install

CMD ["node", "app.js"]
```

---

# Quick Mapping

```text
Manual Step                    Dockerfile

Install Node.js         --->   FROM node:22

cd /app                 --->   WORKDIR /app

Copy Files              --->   COPY . .

npm install             --->   RUN npm install

node app.js             --->   CMD ["node", "app.js"]
```

---

# Understanding COPY . .

Syntax:

```dockerfile
COPY <source> <destination>
```

Example:

```dockerfile
COPY . .
```

means:

```text
COPY current-folder → current-workdir
```

---

# What Does the First Dot Mean?

The first dot is:

```text
Current folder on your computer
```

Suppose:

```text
my-node-app/
|
├── app.js
├── package.json
├── package-lock.json
└── Dockerfile
```

You run:

```bash
docker build -t myapp .
```

The first dot refers to:

```text
my-node-app/
```

---

# What Does the Second Dot Mean?

Suppose:

```dockerfile
WORKDIR /app
```

Then:

```dockerfile
COPY . .
```

means:

```text
Copy everything from

my-node-app/

to

/app

inside the image
```

---

# Visual Example

## On Your Computer

```text
my-node-app/
|
├── app.js
├── package.json
└── Dockerfile
```

Dockerfile:

```dockerfile
FROM node:22

WORKDIR /app

COPY . .
```

---

## Inside the Image

```text
/app
|
├── app.js
├── package.json
└── Dockerfile
```

---

# How Does Docker Know the Current Folder?

From:

```bash
docker build -t myapp .
```

The final dot means:

```text
Use the current directory as the build context
```

Docker sends everything in the current directory to the build process.

---

# Why COPY . . Is Common

Suppose your project contains:

```text
server.js
package.json
src/
config.json
```

Instead of copying files one by one:

```dockerfile
COPY . .
```

copies everything needed.

---

# Potential Problem with COPY . .

It copies everything, including:

```text
node_modules/
.git/
logs/
temporary files
```

This can increase image size.

---

# Solution: .dockerignore

Example:

```text
node_modules
.git
*.log
```

Docker ignores these files during:

```dockerfile
COPY . .
```

---

# Quick Revision

```text
FROM
→ Which runtime do I need?

WORKDIR
→ Where should the application live?

COPY
→ Which files should be copied?

RUN
→ Which setup commands are needed?

CMD
→ How do I start the application?
```

---

# Final Memory Trick

```text
FROM     = Runtime

WORKDIR  = Application Folder

COPY     = Transfer Files

RUN      = Setup / Installation

CMD      = Start Application
```

Mental Model:

```text
Manual Setup
     ↓
Install Runtime
Create Folder
Copy Files
Install Dependencies
Run Application

Dockerfile
     ↓
FROM
WORKDIR
COPY
RUN
CMD
```

If you can manually install and run an application on a fresh machine, you can derive its Dockerfile.