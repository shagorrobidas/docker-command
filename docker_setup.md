# Docker Setup Guide (Beginner Friendly)

Author: Shagor Robidas

This guide will help you install and set up Docker step by step in an easy way.

---

# What is Docker?

Docker is a tool that helps developers run applications inside lightweight containers.

A container includes:
- Application code
- Dependencies
- Libraries
- Runtime environment

This means your app works the same on every machine.

---

# Why Use Docker?

## Benefits of Docker

- Easy project setup
- Same environment everywhere
- Fast deployment
- Lightweight compared to virtual machines
- Great for Django, Node.js, React, MySQL, PostgreSQL, Redis, etc.
- Works well with DevOps and cloud servers

---

# Docker Architecture

```text
Developer → Docker CLI → Docker Engine → Containers
```

## Main Components

| Component | Description |
|---|---|
| Docker CLI | Command line tool |
| Docker Engine | Background service |
| Image | Blueprint/template |
| Container | Running application |
| Docker Hub | Online image storage |

---

# Install Docker on Ubuntu

## Step 1: Update System

```bash
sudo apt update
sudo apt upgrade -y
```

---

## Step 2: Install Required Packages

```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
```

---

## Step 3: Add Docker GPG Key

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

---

## Step 4: Add Docker Repository

```bash
echo \
"deb [arch=$(dpkg --print-architecture) \
signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

---

## Step 5: Update Package List

```bash
sudo apt update
```

---

## Step 6: Install Docker

```bash
sudo apt install docker-ce docker-ce-cli containerd.io -y
```

---

# Check Docker Installation

```bash
docker --version
```

Example Output:

```text
Docker version 27.x.x
```

---

# Start Docker Service

```bash
sudo systemctl start docker
```

Enable Docker at boot:

```bash
sudo systemctl enable docker
```

Check status:

```bash
sudo systemctl status docker
```

---

# Run Docker Without sudo

By default Docker requires sudo.

## Add User to Docker Group

```bash
sudo usermod -aG docker $USER
```

Apply changes:

```bash
newgrp docker
```

Now test:

```bash
docker ps
```

---

# Test Docker Installation

Run hello-world container:

```bash
docker run hello-world
```

If successful, Docker is working correctly.

---

# Install Docker Compose

## Download Docker Compose

```bash
sudo apt install docker-compose-plugin -y
```

Check version:

```bash
docker compose version
```

---

# First Docker Commands

## Pull an Image

```bash
docker pull nginx
```

Explanation:
- Downloads Nginx image from Docker Hub

---

## View Images

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

## Show Running Containers

```bash
docker ps
```

---

## Stop Container

```bash
docker stop container_id
```

---

## Remove Container

```bash
docker rm container_id
```

---

# Understanding Docker Concepts

# 1. Image

An image is like a template.

Example:
- Ubuntu image
- Nginx image
- Python image

---

# 2. Container

A running instance of an image.

Example:

```bash
docker run nginx
```

Creates a running Nginx container.

---

# 3. Dockerfile

A file used to create custom Docker images.

Example:

```Dockerfile
FROM python:3.12

WORKDIR /app

COPY . .

RUN pip install -r requirements.txt

CMD ["python", "app.py"]
```

---

# Create Your First Docker Image

## Step 1: Create Dockerfile

```Dockerfile
FROM ubuntu

CMD ["echo", "Hello Docker"]
```

---

## Step 2: Build Image

```bash
docker build -t myimage .
```

---

## Step 3: Run Image

```bash
docker run myimage
```

Output:

```text
Hello Docker
```

---

# Docker Port Mapping

Applications inside containers use internal ports.

Use `-p` to access from your browser.

Example:

```bash
docker run -d -p 8080:80 nginx
```

Now open:

```text
http://localhost:8080
```

---

# Docker Volumes

Volumes store persistent data.

Example:

```bash
docker volume create mydata
```

Use volume:

```bash
docker run -v mydata:/app/data nginx
```

---

# Docker Networking

Create network:

```bash
docker network create mynetwork
```

Connect container:

```bash
docker network connect mynetwork container
```

---

# Docker Compose

Docker Compose manages multiple containers.

Example:

```yaml
services:
  web:
    image: nginx
    ports:
      - "8080:80"
```

Run:

```bash
docker compose up
```

Stop:

```bash
docker compose down
```

---

# Docker Useful Commands

| Command | Description |
|---|---|
| `docker ps` | Show running containers |
| `docker images` | Show images |
| `docker logs container` | Show logs |
| `docker exec -it container bash` | Open terminal |
| `docker stop container` | Stop container |
| `docker rm container` | Remove container |
| `docker rmi image` | Remove image |
| `docker compose up -d` | Start services |

---

# Docker Cleanup Commands

Remove stopped containers:

```bash
docker container prune
```

Remove unused images:

```bash
docker image prune -a
```

Remove everything unused:

```bash
docker system prune -a
```

---

# Docker Workflow (Real Example)

## Step 1: Pull Python Image

```bash
docker pull python
```

---

## Step 2: Run Python Container

```bash
docker run -it python bash
```

---

## Step 3: Install Packages

```bash
pip install django
```

---

## Step 4: Exit Container

```bash
exit
```

---

# Common Docker Problems

## Permission Denied

Fix:

```bash
sudo usermod -aG docker $USER
newgrp docker
```

---

## Docker Service Not Running

Fix:

```bash
sudo systemctl start docker
```

---

## Port Already Used

Use another port:

```bash
docker run -p 8081:80 nginx
```

---

# Best Practices

- Use official images
- Keep images small
- Remove unused containers
- Use `.dockerignore`
- Use Docker Compose for big projects
- Never store secrets inside images

---

# Official Documentation

Docker Docs:
https://docs.docker.com/

Docker Hub:
https://hub.docker.com/

---

# Conclusion

You now understand:

- What Docker is
- How to install Docker
- How Docker works
- Images and containers
- Docker Compose
- Volumes and networking
- Basic Docker workflow

You are now ready to start Docker projects.

---
