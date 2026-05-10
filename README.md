# Docker Commands Cheat Sheet

A complete beginner-friendly Docker cheat sheet covering the most commonly used Docker commands for:

- Docker Images
- Docker Containers
- Docker Volumes
- Docker Networks
- Docker Compose

---

# 1. Docker Image Commands

Docker images are read-only templates used to create containers.

| Command | Description |
|----------|-------------|
| `docker images` | List all local images |
| `docker images -a` | List all images including intermediate images |
| `docker pull <image>` | Download image from Docker Hub |
| `docker push <image>` | Push image to Docker Hub |
| `docker rmi <image>` | Remove an image |
| `docker rmi -f <image>` | Force remove image |
| `docker build -t <name>:<tag> .` | Build image from Dockerfile |
| `docker buildx build --platform <platform> -t <name> .` | Multi-platform build |
| `docker tag <image> <newname>` | Rename/tag image |
| `docker inspect <image>` | Show image details |
| `docker history <image>` | Show image layer history |
| `docker save -o <file.tar> <image>` | Save image as tar file |
| `docker load -i <file.tar>` | Load image from tar file |
| `docker search <keyword>` | Search Docker Hub images |
| `docker scout <image>` | Check image vulnerabilities |

## Example

```bash
docker pull nginx
docker images
docker run nginx
```

---

# 2. Docker Container Commands

Containers are runnable instances of Docker images.

| Command | Description |
|----------|-------------|
| `docker ps` | List running containers |
| `docker ps -a` | List all containers |
| `docker run <image>` | Run container |
| `docker run -d <image>` | Run container in background |
| `docker run --name <name> <image>` | Run container with custom name |
| `docker start <container>` | Start stopped container |
| `docker stop <container>` | Stop running container |
| `docker restart <container>` | Restart container |
| `docker rm <container>` | Remove container |
| `docker rm -f <container>` | Force remove running container |
| `docker logs <container>` | Show container logs |
| `docker logs -f <container>` | Live stream logs |
| `docker exec -it <container> <cmd>` | Run command inside container |
| `docker inspect <container>` | Show container details |
| `docker top <container>` | Show running processes |
| `docker stats <container>` | Show live resource usage |
| `docker container prune` | Remove stopped containers |
| `docker system prune -a` | Remove unused Docker resources |
| `docker diff <container>` | Show filesystem changes |
| `docker cp <container>:<src> <dest>` | Copy files between container and host |
| `docker rename <old> <new>` | Rename container |

## Example

```bash
docker run -d --name mynginx nginx
docker ps
docker logs mynginx
```

---

# 3. Docker Volume Commands

Volumes are used to persist data outside containers.

| Command | Description |
|----------|-------------|
| `docker volume ls` | List volumes |
| `docker volume create <name>` | Create volume |
| `docker volume inspect <name>` | Show volume details |
| `docker volume rm <name>` | Remove volume |
| `docker volume prune` | Remove unused volumes |
| `docker run -v <vol>:/path <image>` | Mount volume |
| `docker run -v <host_path>:/path <image>` | Mount host directory |
| `docker volume ls -q` | Quiet output |

## Example

```bash
docker volume create mydata
docker run -v mydata:/app/data nginx
```

---

# 4. Docker Network Commands

Docker networks allow containers to communicate.

| Command | Description |
|----------|-------------|
| `docker network ls` | List networks |
| `docker network create <name>` | Create network |
| `docker network inspect <name>` | Show network details |
| `docker network rm <name>` | Remove network |
| `docker network prune` | Remove unused networks |
| `docker network connect <net> <cont>` | Connect container to network |
| `docker network disconnect <net> <cont>` | Disconnect container |
| `docker network ls --filter driver=bridge` | List bridge networks |
| `docker network ls --filter driver=overlay` | List overlay networks |

## Network Drivers

| Driver | Description |
|---------|-------------|
| `bridge` | Default driver for standalone containers |
| `host` | Use host networking |
| `none` | Disable networking |
| `overlay` | Multi-host networking |
| `macvlan` | Assign MAC address to container |

## Example

```bash
docker network create mynetwork
docker network connect mynetwork mycontainer
```

---

# 5. Docker Compose Commands

Docker Compose is used for managing multi-container applications.

| Command | Description |
|----------|-------------|
| `docker compose up` | Build and start services |
| `docker compose up -d` | Start services in detached mode |
| `docker compose down` | Stop and remove services |
| `docker compose ps` | Show running services |
| `docker compose logs` | Show logs |
| `docker compose logs -f` | Follow logs |
| `docker compose build` | Build services |
| `docker compose pull` | Pull service images |
| `docker compose restart` | Restart services |
| `docker compose stop` | Stop services |
| `docker compose start` | Start stopped services |
| `docker compose exec <svc> <cmd>` | Run command inside service |
| `docker compose config` | Validate compose file |
| `docker compose down -v` | Remove volumes |

## Example

```bash
docker compose up -d
docker compose ps
docker compose down
```

---

# Common Docker Workflow

## Pull an Image

```bash
docker pull ubuntu
```

## Run a Container

```bash
docker run -it ubuntu bash
```

## Build an Image

```bash
docker build -t myapp .
```

## Run with Port Mapping

```bash
docker run -p 8000:8000 myapp
```

## Run with Volume

```bash
docker run -v $(pwd):/app myapp
```

---

# Useful Tips

- Use `-d` for detached/background mode
- Use `-it` for interactive terminal access
- Use `docker logs -f` to monitor logs live
- Use Docker Compose for multi-service applications
- Always clean unused resources regularly

---

# Useful Cleanup Commands

```bash
docker system prune
docker volume prune
docker container prune
docker image prune
```

---

# Official Documentation

- Docker Official Docs: https://docs.docker.com/
- Docker Hub: https://hub.docker.com/

---

# License

This project is open-source and free to use.
