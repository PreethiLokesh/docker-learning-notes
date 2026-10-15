# Main Docker Commands 

## Overview

Docker provides commands to:

- Download Images
- Build Images
- Run Containers
- Manage Containers
- View Logs
- Access Containers
- Remove Images and Containers

These are the commands used daily by Docker developers and DevOps engineers.

---

# 1. docker pull

## Purpose

Download an image from a Docker Registry.

Syntax:

```bash
docker pull <image-name>
```

Example:

```bash
docker pull nginx
```

Docker downloads:

```text
nginx:latest
```

from Docker Hub to your local machine.

---

## Pull a Specific Version

```bash
docker pull node:22
```

Docker downloads:

```text
node version 22
```

instead of:

```text
node:latest
```

---

# 2. docker images

## Purpose

Display all images stored locally.

Command:

```bash
docker images
```

Example Output:

```text
REPOSITORY   TAG

nginx        latest

ubuntu       latest

node         22
```

---

# 3. docker run

## Purpose

Create and start a container from an image.

Syntax:

```bash
docker run <image-name>
```

Example:

```bash
docker run nginx
```

Docker performs:

```text
Find Image
     ↓
Create Container
     ↓
Start Container
```

---

## Run in Detached Mode

```bash
docker run -d nginx
```

Meaning:

```text
-d = Detached Mode
```

Container runs in the background.

---

## Run with Name

```bash
docker run --name mynginx nginx
```

Container Name:

```text
mynginx
```

---

## Run with Port Mapping

```bash
docker run -p 8080:80 nginx
```

Meaning:

```text
Host Port 8080
        ↓
Container Port 80
```

---

## Common Real-World Usage

```bash
docker run -d --name mynginx -p 8080:80 nginx
```

---

# 4. docker ps

## Purpose

View running containers.

Command:

```bash
docker ps
```

Example:

```text
CONTAINER ID    IMAGE    STATUS

abc123          nginx    Up 5 minutes
```

---

## View All Containers

```bash
docker ps -a
```

Displays:

```text
Running Containers

Stopped Containers
```

---

# 5. docker stop

## Purpose

Stop a running container.

Command:

```bash
docker stop mynginx
```

Lifecycle:

```text
Running
   ↓
Stopped
```

---

# 6. docker start

## Purpose

Start an existing stopped container.

Command:

```bash
docker start mynginx
```

Lifecycle:

```text
Stopped
    ↓
Running
```

---

# 7. docker restart

## Purpose

Restart a container.

Command:

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

# 8. docker logs

## Purpose

View container logs.

Command:

```bash
docker logs mynginx
```

Logs may include:

```text
Application Output
Errors
Requests
Debug Information
```

---

## Follow Logs in Real Time

```bash
docker logs -f mynginx
```

Meaning:

```text
-f = Follow
```

Exit:

```text
Ctrl + C
```

---

# 9. docker exec

## Purpose

Execute commands inside a running container.

Example:

```bash
docker exec mynginx ls
```

Lists files inside the container.

---

## Open Interactive Shell

```bash
docker exec -it mynginx bash
```

Inside container:

```text
root@container:/#
```

---

## Meaning of Options

### -i

```text
Interactive
```

Keeps input stream open.

---

### -t

```text
Terminal
```

Allocates terminal access.

---

### -it

```text
Interactive Terminal
```

Used when entering containers.

---

# 10. docker rm

## Purpose

Remove a container.

Command:

```bash
docker rm mynginx
```

Container is deleted.

---

## Force Remove

```bash
docker rm -f mynginx
```

Meaning:

```text
-f = Force
```

Docker:

```text
Stop Container
Delete Container
```

in a single command.

---

# 11. docker rmi

## Purpose

Remove a Docker Image.

Command:

```bash
docker rmi nginx
```

Image removed from local storage.

---

## Important Rule

Docker cannot remove an image that is being used by a container.

First remove containers:

```bash
docker rm mycontainer
```

Then remove image:

```bash
docker rmi nginx
```

---

# 12. docker build

## Purpose

Build an image from a Dockerfile.

Syntax:

```bash
docker build -t image-name .
```

Example:

```bash
docker build -t myapp:v1 .
```

---

## Understanding the Command

```text
docker build
       ↓
Build Image

-t
 ↓
Tag

myapp
 ↓
Image Name

v1
 ↓
Version

.
 ↓
Current Directory
```

---

# 13. docker inspect

## Purpose

View detailed configuration of an image or container.

Command:

```bash
docker inspect mynginx
```

Information Available:

```text
IP Address
Ports
Volumes
Networks
Environment Variables
Configuration
```

---

# 14. docker stats

## Purpose

Monitor Docker container resource usage.

Command:

```bash
docker stats
```

Displays:

```text
CPU Usage
Memory Usage
Network Usage
```

Useful in production environments.

---

# Most Important Docker Commands

```bash
docker pull nginx

docker images

docker run nginx

docker run -d nginx

docker run --name mynginx nginx

docker run -p 8080:80 nginx

docker ps

docker ps -a

docker stop mynginx

docker start mynginx

docker restart mynginx

docker logs mynginx

docker logs -f mynginx

docker exec -it mynginx bash

docker rm mynginx

docker rm -f mynginx

docker rmi nginx

docker build -t myapp:v1 .

docker inspect mynginx

docker stats
```

---

# Real Docker Workflow

```text
docker pull nginx
           ↓

docker run -d --name mynginx -p 8080:80 nginx
           ↓

docker ps
           ↓

docker logs mynginx
           ↓

docker exec -it mynginx bash
           ↓

docker stop mynginx
           ↓

docker rm mynginx
```

---

# Interview Questions

## What does docker pull do?

Downloads an image from a registry to the local machine.

---

## What does docker run do?

Creates and starts a container from an image.

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

## Difference Between docker rm and docker rmi?

docker rm:

```text
Remove Container
```

docker rmi:

```text
Remove Image
```

---

## What does docker exec -it do?

Provides interactive terminal access inside a running container.

---

## What does docker logs -f do?

Displays logs continuously in real time.

---

## What does -d mean?

```text
Detached Mode
```

Runs container in the background.

---

## What does -p mean?

```text
Port Mapping
```

Format:

```text
HostPort:ContainerPort
```

Example:

```bash
-p 8080:80
```

---

# Quick Revision

```text
docker pull
→ Download Image

docker images
→ List Images

docker build
→ Build Image

docker run
→ Create + Start Container

docker ps
→ Running Containers

docker ps -a
→ All Containers

docker stop
→ Stop Container

docker start
→ Start Existing Container

docker restart
→ Restart Container

docker logs
→ View Logs

docker logs -f
→ Follow Logs

docker exec -it
→ Enter Container

docker rm
→ Remove Container

docker rm -f
→ Force Remove Container

docker rmi
→ Remove Image

docker inspect
→ Container Details

docker stats
→ Resource Monitoring
```

---

# Memory Trick

```text
pull     → Download

build    → Create Image

run      → Create + Start Container

ps       → Show Containers

logs     → Read Logs

exec     → Enter Container

stop     → Stop

start    → Start

rm       → Remove Container

rmi      → Remove Image
```