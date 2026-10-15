# Image Versioning (Tags)

## Overview

Docker Images can have different versions.

Docker uses **Tags** to identify image versions.

A tag is a label attached to an image.

General Format:

```text
image-name:tag
```

Examples:

```text
nginx:latest

node:22

ubuntu:24.04

myapp:v1
```

---

# What is a Docker Tag?

A Tag is a version identifier attached to a Docker Image.

Format:

```text
image-name:tag
```

Example:

```text
node:22
```

Breakdown:

```text
node
 ↓
Image Name

22
 ↓
Tag (Version)
```

---

# Why Do We Need Tags?

Without tags:

```text
Only one image version exists
```

With tags:

```text
myapp:v1

myapp:v2

myapp:v3
```

Multiple versions can exist simultaneously.

---

# Understanding Image Names and Tags

Example:

```text
ubuntu:24.04
```

```text
ubuntu
    ↓
Image Name

24.04
    ↓
Tag
```

Another Example:

```text
postgres:17
```

```text
postgres
      ↓
Image Name

17
 ↓
Tag
```

---

# What is the latest Tag?

When no tag is specified:

```bash
docker pull nginx
```

Docker automatically uses:

```text
nginx:latest
```

Equivalent to:

```bash
docker pull nginx:latest
```

---

# Important Misconception

Many beginners think:

```text
latest = newest version
```

This is NOT always true.

`latest` is simply another tag.

Example:

```text
myapp:latest
```

means:

```text
Default version selected by developer
```

It does NOT guarantee the newest release.

---

# Why Avoid latest in Production?

Assume today:

```text
node:latest
→ Node 22
```

A few months later:

```text
node:latest
→ Node 23
```

Applications may behave differently.

This can cause:

```text
Unexpected failures

Deployment issues

Compatibility problems
```

---

# Best Practice

Avoid:

```bash
docker pull node:latest
```

Prefer:

```bash
docker pull node:22
```

Now every deployment consistently uses:

```text
Node Version 22
```

---

# Creating Tagged Images

Build an image:

```bash
docker build -t myapp:v1 .
```

Image created:

```text
myapp:v1
```

---

Create another version:

```bash
docker build -t myapp:v2 .
```

Now both versions exist:

```text
myapp:v1

myapp:v2
```

---

# Listing Tagged Images

Command:

```bash
docker images
```

Example:

```text
REPOSITORY   TAG

myapp        v1

myapp        v2

nginx        latest
```

---

# Running a Specific Version

Run Version 1:

```bash
docker run myapp:v1
```

Run Version 2:

```bash
docker run myapp:v2
```

Docker runs the exact requested version.

---

# Real-World Example

Version 1:

```text
Login Feature
```

Build:

```bash
docker build -t myapp:v1 .
```

---

Version 2:

```text
Login + Signup Feature
```

Build:

```bash
docker build -t myapp:v2 .
```

---

Version 3:

```text
Login + Signup + Payment Feature
```

Build:

```bash
docker build -t myapp:v3 .
```

Images:

```text
myapp:v1
myapp:v2
myapp:v3
```

---

# Why Versioning is Important

Suppose:

```text
myapp:v3
```

contains a bug.

Instead of rebuilding immediately:

```bash
docker run myapp:v2
```

The previous stable version can be deployed.

This process is called:

```text
Rollback
```

---

# Tagging an Existing Image

Suppose:

```text
myapp:v1
```

already exists.

Create another tag:

```bash
docker tag myapp:v1 myapp:latest
```

Now:

```text
myapp:v1

myapp:latest
```

both point to the same image.

---

# Visual Representation

```text
Image ID
   |
   +---- myapp:v1
   |
   +---- myapp:latest
```

Multiple tags can point to the same image.

---

# Pushing Tagged Images

Push Version 1:

```bash
docker push username/myapp:v1
```

Push Version 2:

```bash
docker push username/myapp:v2
```

Registry stores each tag separately.

---

# Versioning Strategies

## Strategy 1: Simple Versioning

Examples:

```text
myapp:v1

myapp:v2

myapp:v3
```

Simple and beginner friendly.

---

## Strategy 2: Semantic Versioning

Format:

```text
MAJOR.MINOR.PATCH
```

Examples:

```text
1.0.0

1.1.0

1.1.1

2.0.0
```

---

# Major Version

Breaking changes.

Example:

```text
1.0.0 → 2.0.0
```

---

# Minor Version

New features added.

Example:

```text
1.0.0 → 1.1.0
```

---

# Patch Version

Bug fixes only.

Example:

```text
1.1.0 → 1.1.1
```

---

# Common Real-World Tags

```text
node:22

python:3.12

postgres:17

ubuntu:24.04

nginx:latest
```

---

# Commands Used for Versioning

Build Version 1:

```bash
docker build -t myapp:v1 .
```

---

Build Version 2:

```bash
docker build -t myapp:v2 .
```

---

List Images:

```bash
docker images
```

---

Run Specific Version:

```bash
docker run myapp:v1
```

---

Create Additional Tag:

```bash
docker tag myapp:v1 myapp:latest
```

---

Push Version:

```bash
docker push username/myapp:v1
```

---

# Interview Questions

## What is a Docker Tag?

A Docker Tag is a version identifier attached to a Docker Image.

Example:

```text
node:22
```

Tag:

```text
22
```

---

## Why are Tags Important?

Tags help:

```text
Track versions

Deploy specific releases

Enable rollbacks

Prevent unexpected changes
```

---

## What is the latest Tag?

A default Docker tag used when no tag is specified.

Example:

```bash
docker pull nginx
```

Actually pulls:

```text
nginx:latest
```

---

## Why Should latest Be Avoided in Production?

Because the underlying image version may change over time, leading to inconsistent deployments.

Prefer:

```text
node:22

ubuntu:24.04

myapp:v1
```

---

## What Does docker tag Do?

Creates another tag pointing to the same image.

Example:

```bash
docker tag myapp:v1 myapp:latest
```

---

# Quick Revision

```text
Format:

image:tag

Examples:

node:22

ubuntu:24.04

postgres:17

myapp:v1

Tag
→ Version Identifier

latest
→ Default Tag

Best Practice
→ Use Explicit Versions

Good:
node:22

Bad:
node:latest

Build:
docker build -t myapp:v1 .

Run:
docker run myapp:v1

Tag Existing Image:
docker tag myapp:v1 myapp:latest

Rollback:
myapp:v3 → Bug
Deploy myapp:v2
```

---

# Memory Trick

```text
Image Name
        :
       Tag

node:22

ubuntu:24.04

myapp:v1
```

Remember:

```text
Tag = Version

Version = Control

Control = Safe Deployments
```
``