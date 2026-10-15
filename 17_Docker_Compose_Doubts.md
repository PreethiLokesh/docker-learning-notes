# Docker Compose - Doubt Notes

## Doubt 1: Why are there two `nginx` values here?

Example:

```yaml
services:
  nginx:
    image: nginx
```

At first glance it looks like the same thing is written twice.

---

### First nginx

```yaml
services:
  nginx:
```

This is the **Service Name**.

It is a name chosen by the developer.

Examples:

```yaml
services:
  webserver:
```

```yaml
services:
  frontend:
```

```yaml
services:
  my-nginx:
```

All are valid.

---

### Second nginx

```yaml
image: nginx
```

This is the actual Docker image.

Equivalent to:

```bash
docker pull nginx
```

Docker uses this image to create containers.

---

### Example

```yaml
services:
  webserver:
    image: nginx
```

Meaning:

```text
Service Name = webserver

Docker Image = nginx
```

---

### Memory Trick

```yaml
services:

  service-name:
    image: image-name
```

Example:

```yaml
services:

  backend:
    image: node:22
```

```text
backend
    ↓
Service Name

node:22
    ↓
Docker Image
```

---

# Doubt 2: Why are volumes and networks written separately?

Example:

```yaml
services:

  mysql:
    image: mysql

    volumes:
      - mysql-data:/var/lib/mysql

    networks:
      - app-net

volumes:
  mysql-data:

networks:
  app-net:
```

---

### Why Not Put Everything Inside Services?

Because Docker Compose separates:

```text
Resource Creation

and

Resource Usage
```

---

## Top-Level Sections

These sections create resources.

```yaml
volumes:
  mysql-data:

networks:
  app-net:
```

Meaning:

```text
Create Volume:
mysql-data

Create Network:
app-net
```

---

## Service Sections

These sections use resources.

```yaml
volumes:
  - mysql-data:/var/lib/mysql

networks:
  - app-net
```

Meaning:

```text
Attach mysql-data volume

Attach app-net network
```

to the container.

---

### Memory Trick

```text
Top-Level
---------
volumes:
networks:

= CREATE

Service-Level
-------------
volumes:
networks:

= USE
```

---

# Doubt 3: What does the hyphen (-) mean?

Example:

```yaml
networks:
  - app-net
```

or

```yaml
volumes:
  - mysql-data:/var/lib/mysql
```

---

## Meaning of -

In YAML:

```text
- = List Item
```

Similar to:

```text
Shopping List

- Milk
- Bread
- Butter
```

---

## Example

```yaml
networks:
  - app-net
```

Meaning:

```text
Network List

1. app-net
```

---

## Multiple Networks

```yaml
networks:
  - frontend-net
  - database-net
```

Meaning:

```text
1. frontend-net

2. database-net
```

Container joins both networks.

---

## Multiple Volumes

```yaml
volumes:
  - mysql-data:/var/lib/mysql
  - logs-data:/logs
```

Meaning:

```text
mysql-data → /var/lib/mysql

logs-data → /logs
```

---

### Memory Trick

```text
- = Bullet Point

- = Array Element

- = List Item
```

---

# Doubt 4: Why is the port defined for backend but not for MySQL?

Example:

```yaml
services:

  backend:
    ports:
      - "3000:3000"

  mysql:
    image: mysql
```

---

## Backend Port Mapping

```yaml
ports:
  - "3000:3000"
```

Meaning:

```text
Host Port 3000
        ↓
Container Port 3000
```

This allows:

```text
Browser
        ↓
localhost:3000
        ↓
Backend Container
```

---

## Why MySQL Doesn't Need Ports Here

Backend and MySQL are already on the same Docker network.

```text
backend
    ↓
app-net
    ↓
mysql
```

Backend can connect using:

```text
mysql:3306
```

No host access required.

---

## When Should MySQL Ports Be Exposed?

If external tools need access.

Example:

```yaml
mysql:
  image: mysql

  ports:
    - "3306:3306"
```

Now tools like:

```text
MySQL Workbench

DBeaver

TablePlus
```

can connect using:

```text
localhost:3306
```

---

### Real-World Practice

Usually:

```text
Frontend
    ↓
Expose Port

Backend
    ↓
Expose Port

Database
    ↓
Do NOT Expose Port
```

Unless external access is required.

---

### Memory Trick

```text
Ports
     ↓
Host ↔ Container

Networks
     ↓
Container ↔ Container
```

---

# Final Quick Revision

## Service Name vs Image

```yaml
services:
  backend:
    image: node:22
```

```text
backend
    ↓
Service Name

node:22
    ↓
Docker Image
```

---

## Top-Level vs Service-Level

```yaml
volumes:
  mysql-data:
```

```text
Create Volume
```

---

```yaml
volumes:
  - mysql-data:/var/lib/mysql
```

```text
Use Volume
```

---

## Hyphen (-)

```yaml
- app-net
```

```text
List Item
```

---

## Ports

```yaml
ports:
  - "3000:3000"
```

```text
Host ↔ Container
```

---

## Networks

```yaml
networks:
  - app-net
```

```text
Container ↔ Container
```

---

## Golden Rule

```text
Ports
      ↓
Access Container From Outside

Networks
      ↓
Connect Containers Together

Volumes
      ↓
Persist Data
```