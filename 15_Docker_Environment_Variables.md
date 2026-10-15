# Docker Environment Variables 

## Overview

Environment Variables allow applications to receive configuration values without modifying source code.

Instead of hardcoding values inside an application, Docker can provide them at runtime.

---

# What Problem Do Environment Variables Solve?

Bad Practice:

```javascript
const dbUser = "admin";
const dbPassword = "password123";
```

Problems:

```text
Password Visible in Code

Different Code for Different Environments

Hard to Maintain

Security Risk
```

---

# Better Approach

Use Environment Variables.

Example:

```javascript
const dbUser = process.env.DB_USER;
const dbPassword = process.env.DB_PASSWORD;
```

Now values come from Docker instead of source code.

---

# What is an Environment Variable?

An Environment Variable is a key-value pair available inside a container.

Examples:

```text
DB_USER=admin

DB_PASSWORD=secret

APP_ENV=production
```

---

# Why Use Environment Variables?

They help separate:

```text
Code

Configuration
```

This allows the same application code to run in:

```text
Development

Testing

Production
```

with different settings.

---

# Method 1: Using -e in docker run

Syntax:

```bash
docker run -e VARIABLE_NAME=value image_name
```

Example:

```bash
docker run -e APP_ENV=production nginx
```

Creates:

```text
APP_ENV=production
```

inside the container.

---

# Multiple Environment Variables

```bash
docker run \
-e DB_USER=admin \
-e DB_PASSWORD=secret \
-e APP_ENV=production \
nginx
```

Variables Available:

```text
DB_USER=admin

DB_PASSWORD=secret

APP_ENV=production
```

---

# Viewing Environment Variables

Enter Container:

```bash
docker exec -it mycontainer bash
```

Display Variables:

```bash
env
```

Output Example:

```text
DB_USER=admin

DB_PASSWORD=secret

APP_ENV=production
```

---

# Environment Variables in Applications

Node.js Example:

```javascript
console.log(process.env.APP_ENV);
```

Run:

```bash
docker run -e APP_ENV=production myapp
```

Output:

```text
production
```

---

# Method 2: Using .env Files

For many variables, writing multiple `-e` options becomes difficult.

Instead create:

```text
.env
```

---

# Example .env File

```env
DB_HOST=mysql
DB_USER=admin
DB_PASSWORD=secret
APP_ENV=production
```

---

# Run Using .env File

```bash
docker run --env-file .env myapp
```

Docker reads:

```text
.env
     ↓
Container Variables
```

---

# Advantages of .env Files

```text
Cleaner Configuration

Easy Maintenance

Supports Many Variables

Common in Real Projects
```

---

# Method 3: Using ENV in Dockerfile

Environment Variables can also be defined inside a Dockerfile.

Example:

```dockerfile
FROM node:22

ENV APP_ENV=development

CMD ["node", "app.js"]
```

---

# What Happens?

Every container started from this image gets:

```text
APP_ENV=development
```

by default.

---

# Checking Variables

Inside Container:

```bash
env
```

Output:

```text
APP_ENV=development
```

---

# Overriding Dockerfile ENV Values

Dockerfile:

```dockerfile
ENV APP_ENV=development
```

Run:

```bash
docker run -e APP_ENV=production myapp
```

Result:

```text
APP_ENV=production
```

Runtime value overrides Dockerfile value.

---

# Real Project Structure

```text
project/
|
├── app.js
├── Dockerfile
└── .env
```

---

# Dockerfile

```dockerfile
FROM node:22

WORKDIR /app

COPY . .

CMD ["node", "app.js"]
```

---

# .env

```env
DB_HOST=mysql
DB_USER=admin
DB_PASSWORD=secret
APP_ENV=production
```

---

# Run Application

```bash
docker run --env-file .env myapp
```

Application can access:

```javascript
process.env.DB_HOST

process.env.DB_USER

process.env.DB_PASSWORD

process.env.APP_ENV
```

---

# Real-World Examples

## Database Configuration

```text
DB_HOST=mysql

DB_PORT=3306

DB_USER=admin

DB_PASSWORD=secret
```

---

## Application Settings

```text
APP_ENV=production

APP_PORT=3000
```

---

## API Configuration

```text
API_KEY=abcdef123

API_URL=https://example.com
```

---

# Example: MySQL Container

Run:

```bash
docker run -d \
--name mysql \
-e MYSQL_ROOT_PASSWORD=secret \
mysql
```

Variable Created:

```text
MYSQL_ROOT_PASSWORD=secret
```

MySQL uses it to create the root password.

---

# Why Environment Variables Matter

Without Environment Variables:

```text
Different Code for Development

Different Code for Testing

Different Code for Production
```

Bad practice.

---

With Environment Variables:

```text
Same Code

Different Configuration
```

Best practice.

---

# Example Across Environments

## Development

```text
APP_ENV=development

DB_HOST=dev-db
```

---

## Testing

```text
APP_ENV=test

DB_HOST=test-db
```

---

## Production

```text
APP_ENV=production

DB_HOST=prod-db
```

Application code remains unchanged.

---

# Which Method Should Be Used?

## Few Variables

Use:

```bash
docker run -e
```

Example:

```bash
docker run -e APP_ENV=production nginx
```

---

## Many Variables

Use:

```text
.env file
```

Example:

```bash
docker run --env-file .env myapp
```

---

## Default Values

Use:

```dockerfile
ENV
```

Example:

```dockerfile
ENV APP_ENV=development
```

---

# Environment Variables vs Volumes vs Networks

## Environment Variables

Purpose:

```text
Configuration
```

Example:

```bash
-e DB_USER=admin
```

---

## Volumes

Purpose:

```text
Data Persistence
```

Example:

```bash
-v mydata:/data
```

---

## Networks

Purpose:

```text
Container Communication
```

Example:

```bash
--network app-net
```

---

# Important Commands

Single Variable:

```bash
docker run -e APP_ENV=production nginx
```

---

Multiple Variables:

```bash
docker run \
-e DB_USER=admin \
-e DB_PASSWORD=secret \
nginx
```

---

Using .env File:

```bash
docker run --env-file .env myapp
```

---

View Variables:

```bash
env
```

---

# Interview Questions

## What Are Environment Variables?

Environment Variables are runtime configuration values provided to applications without modifying source code.

---

## Why Use Environment Variables?

To separate:

```text
Configuration

Secrets

Credentials

Application Settings
```

from application code.

---

## What Does -e Mean?

Example:

```bash
docker run -e APP_ENV=production nginx
```

Creates:

```text
APP_ENV=production
```

inside the container.

---

## What Is a .env File?

A file containing environment variables that can be loaded into a container.

Example:

```env
DB_USER=admin
DB_PASSWORD=secret
```

---

## Difference Between ENV and -e?

ENV:

```text
Defined in Dockerfile

Default Value
```

Example:

```dockerfile
ENV APP_ENV=development
```

---

-e:

```text
Provided During docker run

Overrides ENV
```

Example:

```bash
docker run -e APP_ENV=production myapp
```

---

# Quick Revision

```text
Environment Variables
          ↓
Application Configuration

Methods:

1. -e

2. .env File

3. ENV in Dockerfile

Examples:

DB_USER=admin

DB_PASSWORD=secret

APP_ENV=production

Commands:

docker run -e APP_ENV=production nginx

docker run --env-file .env myapp

Dockerfile:

ENV APP_ENV=development
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

Environment Variables
       ↓
Configure Applications
```

---

# Final Summary

```text
Few Variables
        ↓
Use -e

Many Variables
        ↓
Use .env File

Default Variables
        ↓
Use ENV in Dockerfile
```

Environment Variables allow the same application code to be used in development, testing, staging, and production environments by changing configuration instead of changing code.s