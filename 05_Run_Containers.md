# Run Containers 

## Overview

A Docker Container is a running instance of a Docker Image.

This section covers:

- Pulling Images
- Running Containers
- Detached Mode
- Port Mapping
- Container Lifecycle
- Logs
- Executing Commands Inside Containers
- Container Management Commands

---

# What is a Container?

A Container is a running instance of a Docker Image.

```text
Image
  |
docker run
  |
  v
Container
```

Example:

```bash
docker run nginx
```

Docker creates and starts an nginx container.

---

# Pull an Image

Before running a container, Docker requires an image.

Download an image:

```bash
docker pull nginx
```

Example Output:

```text
Using default tag: latest
latest: Pulling from library/nginx
Download complete
```

---

# List Images

Check downloaded images:

```bash
docker images
```

Example:

```text
REPOSITORY   TAG      IMAGE ID

nginx        latest   abc123
ubuntu       latest   xyz789
```

---

# Run a Container

Syntax:

```bash
docker run <image-name>
```

Example:

```bash
docker run nginx
```

Docker:

1. Locates Image
2. Creates Container
3. Starts Container

---

# Detached Mode (-d)

Normally:

```bash
docker run nginx
```

keeps the terminal occupied.

Run in background:

```bash
docker run -d nginx
```

Example Output:

```text
7c4ab157c1ae...
```

Terminal becomes available again.

---

# List Running Containers

Show active containers:

```bash
docker ps
```

Example:

```text
CONTAINER ID   IMAGE   STATUS

7c4ab157c1ae   nginx   Up 2 minutes
```

---

# List All Containers

Show running and stopped containers:

```bash
docker ps -a
```

Example:

```text
CONTAINER ID   IMAGE    STATUS

abc123         nginx    Up

xyz456         ubuntu   Exited
```

---

# Container Naming

Without a name:

```bash
docker run nginx
```

Docker generates names such as:

```text
happy_turing

friendly_tesla

hungry_einstein
```

Assign your own name:

```bash
docker run --name mynginx nginx
```

---

# Port Mapping (-p)

Containers run on internal ports.

To access them from outside:

```bash
docker run -d -p 8080:80 nginx
```

Meaning:

```text
Host Port        Container Port

8080       -->        80
```

---

# How Port Mapping Works

```text
Browser
   |
localhost:8080
   |
   v
Container Port 80
```

Open:

```text
http://localhost:8080
```

Expected Page:

```text
Welcome to nginx!
```

---

# More Port Mapping Examples

## Node.js Application

```bash
docker run -p 3000:3000 myapp
```

---

## Flask Application

```bash
docker run -p 5000:5000 flaskapp
```

---

## MongoDB

```bash
docker run -p 27017:27017 mongo
```

---

## MySQL

```bash
docker run -p 3306:3306 mysql
```

---

# Stop a Container

By Name:

```bash
docker stop mynginx
```

By Container ID:

```bash
docker stop 7c4ab157
```

---

# Start a Container

Start a previously stopped container:

```bash
docker start mynginx
```

---

# Restart a Container

```bash
docker restart mynginx
```

Equivalent to:

```text
Stop
 ↓
Start
```

---

# Remove a Container

First stop:

```bash
docker stop mynginx
```

Then remove:

```bash
docker rm mynginx
```

Container deleted.

---

# Force Remove a Container

Remove even if it is running:

```bash
docker rm -f mynginx
```

Explanation:

```text
-f = Force
```

---

# View Logs

Check container output:

```bash
docker logs mynginx
```

Logs include:

- Application Output
- Errors
- Request Information

---

# Follow Logs Continuously

```bash
docker logs -f mynginx
```

Meaning:

```text
-f = Follow
```

Displays logs in real time.

Exit:

```text
Ctrl + C
```

---

# Execute Commands Inside a Container

Open a shell:

```bash
docker exec -it mynginx bash
```

Inside Container:

```text
root@container:/#
```

Example Commands:

```bash
ls

pwd

whoami
```

Exit:

```bash
exit
```

---

# Meaning of Options

## -i

Interactive Mode

Keeps input stream open.

---

## -t

Terminal Mode

Provides terminal access.

---

## -it

Interactive Terminal

Used for shell access.

Example:

```bash
docker exec -it mynginx bash
```

---

# Ubuntu Container Example

Run:

```bash
docker run ubuntu
```

Container exits immediately.

---

# Why Does Ubuntu Exit?

Containers remain active only while their main process runs.

Ubuntu image has no foreground process.

Therefore:

```text
Start
 ↓
Exit
```

---

# Run Ubuntu Interactively

```bash
docker run -it ubuntu bash
```

Now you're inside Ubuntu.

Check OS:

```bash
cat /etc/os-release
```

List Files:

```bash
ls
```

Exit:

```bash
exit
```

Container stops.

---

# Important Commands Summary

## Pull Image

```bash
docker pull nginx
```

---

## List Images

```bash
docker images
```

---

## Run Container

```bash
docker run nginx
```

---

## Run in Background

```bash
docker run -d nginx
```

---

## Assign Name

```bash
docker run --name mynginx nginx
```

---

## Port Mapping

```bash
docker run -p 8080:80 nginx
```

---

## Running Containers

```bash
docker ps
```

---

## All Containers

```bash
docker ps -a
```

---

## Stop Container

```bash
docker stop mynginx
```

---

## Start Container

```bash
docker start mynginx
```

---

## Restart Container

```bash
docker restart mynginx
```

---

## View Logs

```bash
docker logs mynginx
```

---

## Follow Logs

```bash
docker logs -f mynginx
```

---

## Execute Shell

```bash
docker exec -it mynginx bash
```

---

## Remove Container

```bash
docker rm mynginx
```

---

## Force Remove

```bash
docker rm -f mynginx
```

---

# Container Lifecycle

```text
docker pull nginx
         ↓

docker run nginx
         ↓

Running Container
         ↓

docker stop
         ↓

Stopped Container
         ↓

docker start
         ↓

Running Container
         ↓

docker rm
         ↓

Container Deleted
```

---

# Interview Questions

## What does docker run do?

Creates and starts a new container from an image.

---

## Difference Between docker run and docker start?

docker run:

```text
Create + Start Container
```

docker start:

```text
Start Existing Container
```

---

## What is Detached Mode?

```text
-d
```

Runs the container in the background.

---

## What is Port Mapping?

```text
-p HOST_PORT:CONTAINER_PORT
```

Example:

```bash
-p 8080:80
```

Maps host port 8080 to container port 80.

---

## What does docker ps show?

Shows currently running containers.

---

## What does docker ps -a show?

Shows all containers including stopped containers.

---

# Quick Revision

```text
docker pull
    ↓
Download Image

docker run
    ↓
Create and Start Container

docker ps
    ↓
Running Containers

docker ps -a
    ↓
All Containers

docker stop
    ↓
Stop Container

docker start
    ↓
Start Existing Container

docker restart
    ↓
Restart Container

docker logs
    ↓
View Logs

docker exec -it
    ↓
Enter Container

docker rm
    ↓
Delete Container

-p
    ↓
Port Mapping

-d
    ↓
Detached Mode

-f
    ↓
Force / Follow (depends on command)
```