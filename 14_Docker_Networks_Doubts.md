# Docker Networks - Doubt 

## Doubt: Do We Need to Attach Networks to Containers All the Time?

### Short Answer

No.

Docker automatically attaches every container to a network.

---

# What Happens Normally?

Run:

```bash
docker run nginx
```

Docker automatically connects it to:

```text
bridge network
```

You did not specify:

```bash
--network
```

but networking still works.

---

# Default Behavior

```text
nginx Container
        ↓
Bridge Network
```

Docker handles this automatically.

---

# When Is Default Network Enough?

Examples:

```bash
docker run nginx
```

```bash
docker run ubuntu
```

```bash
docker run my-python-app
```

These are single containers.

They do not need communication with other containers.

Default bridge network is enough.

---

# Single Container Scenario

```text
Nginx Container
```

Need Custom Network?

```text
No
```

Reason:

```text
No Other Containers To Talk To
```

---

# Ubuntu Container Scenario

```bash
docker run -it ubuntu bash
```

Need Custom Network?

```text
No
```

Reason:

```text
Used For Practice/Learning
```

---

# When Do We Need a Custom Network?

Ask:

```text
Does this container need to talk to another container?
```

If YES:

```text
Use a Custom Network
```

---

# Example

Application:

```text
Frontend

Backend

Database
```

Containers:

```text
frontend-container

backend-container

mysql-container
```

Communication Needed:

```text
Frontend → Backend

Backend → Database
```

Now create:

```bash
docker network create app-net
```

Attach:

```bash
docker run --network app-net ...
```

---

# Example Architecture

```text
Frontend Container
         |
         |
         v
      app-net
         |
         |
         v
Backend Container
         |
         |
         v
      app-net
         |
         |
         v
MySQL Container
```

All communicate through:

```text
app-net
```

---

# Real-World Rule

## Single Container

```bash
docker run nginx
```

Need Custom Network?

```text
No
```

---

## Frontend + Backend

```text
React

Node.js
```

Need Custom Network?

```text
Yes
```

---

## Backend + Database

```text
Node.js

MySQL
```

Need Custom Network?

```text
Yes
```

---

## Microservices

```text
Auth Service

User Service

Payment Service

Order Service

Database
```

Need Custom Network?

```text
Definitely Yes
```

---

# Comparison With Volumes

Volumes Question:

```text
Do I need data persistence?
```

If YES:

```text
Use Volume
```

---

Networks Question:

```text
Do containers need communication?
```

If YES:

```text
Use Custom Network
```

---

# Industry Example

Application:

```text
Frontend
Backend
MySQL
```

Architecture:

```text
                app-network
                      |
      --------------------------------
      |              |              |
      v              v              v

  Frontend      Backend         MySQL
                                    |
                                    v
                             mysql-data
```

Notice:

```text
One Network

One Volume
```

Network:

```text
Communication
```

Volume:

```text
Storage
```

---

# Interview Question

## Does Every Container Need a Custom Network?

No.

Every container automatically gets attached to Docker's default bridge network.

Custom networks are used only when multiple containers need reliable communication.

---

# Quick Revision

```text
Every Container Gets:

Default Bridge Network
```

Examples:

```bash
docker run nginx

docker run ubuntu
```

No custom network required.

---

Use Custom Networks When:

```text
Frontend ↔ Backend

Backend ↔ Database

Microservices
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
```

And:

```text
Single Container
       ↓
Default Bridge Network

Multiple Containers
       ↓
Custom Network
```
``