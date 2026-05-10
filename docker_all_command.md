# Docker Complete Commands Guide

Author: Shagor Robidas

A complete Docker command reference from beginner to advanced level for developers and DevOps engineers.

---

# 1. Docker Basics

| Command | Explanation |
|---|---|
| `docker --version` | Show installed Docker version |
| `docker version` | Show detailed Docker client/server version |
| `docker info` | Show Docker system information |
| `docker help` | Show all Docker commands |
| `docker <command> --help` | Show help for a specific command |

---

# 2. Docker Image Commands

Docker images are templates used to create containers.

| Command | Explanation |
|---|---|
| `docker images` | List all local images |
| `docker image ls` | Alternative command to list images |
| `docker images -a` | Show all images including intermediate layers |
| `docker search nginx` | Search images from Docker Hub |
| `docker pull nginx` | Download image from Docker Hub |
| `docker pull ubuntu:22.04` | Pull a specific image version |
| `docker push username/image` | Upload image to Docker Hub |
| `docker rmi image_id` | Remove image |
| `docker rmi -f image_id` | Force remove image |
| `docker build -t myapp .` | Build image from Dockerfile |
| `docker build -t myapp:v1 .` | Build image with version tag |
| `docker buildx build --platform linux/amd64 -t myapp .` | Build multi-platform image |
| `docker tag myapp username/myapp:v1` | Create another tag for image |
| `docker inspect nginx` | Show detailed image information |
| `docker history nginx` | Show image layer history |
| `docker save -o backup.tar nginx` | Save image to tar file |
| `docker load -i backup.tar` | Load image from tar file |
| `docker commit container myimage` | Create image from running container |
| `docker import backup.tar myimage` | Import image from tar archive |
| `docker image prune` | Remove dangling images |
| `docker image prune -a` | Remove all unused images |

---

# 3. Docker Container Commands

Containers are running instances of Docker images.

| Command | Explanation |
|---|---|
| `docker run nginx` | Run container from image |
| `docker run -d nginx` | Run container in background |
| `docker run -it ubuntu bash` | Start interactive terminal |
| `docker run --name web nginx` | Run container with custom name |
| `docker run -p 8080:80 nginx` | Map host port to container port |
| `docker run -e DEBUG=True myapp` | Set environment variable |
| `docker run -v mydata:/data nginx` | Mount Docker volume |
| `docker run --restart always nginx` | Auto restart container |
| `docker run --memory=512m nginx` | Limit memory usage |
| `docker run --cpus=1 nginx` | Limit CPU usage |
| `docker ps` | Show running containers |
| `docker ps -a` | Show all containers |
| `docker start container` | Start stopped container |
| `docker stop container` | Stop running container |
| `docker restart container` | Restart container |
| `docker pause container` | Pause container processes |
| `docker unpause container` | Resume paused container |
| `docker kill container` | Force stop container |
| `docker rm container` | Remove container |
| `docker rm -f container` | Force remove running container |
| `docker rename old new` | Rename container |
| `docker attach container` | Attach terminal to running container |
| `docker exec -it container bash` | Open shell inside container |
| `docker exec -it container sh` | Open sh shell inside container |
| `docker logs container` | Show container logs |
| `docker logs -f container` | Live stream container logs |
| `docker logs --tail 100 container` | Show last 100 log lines |
| `docker top container` | Show running processes |
| `docker stats` | Show live resource usage |
| `docker inspect container` | Show container details |
| `docker cp file.txt container:/app` | Copy file into container |
| `docker cp container:/app/file.txt .` | Copy file from container |
| `docker diff container` | Show changed files |
| `docker wait container` | Wait until container stops |
| `docker port container` | Show mapped ports |
| `docker container prune` | Remove stopped containers |

---

# 4. Docker Volume Commands

Volumes store persistent container data.

| Command | Explanation |
|---|---|
| `docker volume ls` | List all volumes |
| `docker volume create myvolume` | Create volume |
| `docker volume inspect myvolume` | Show volume details |
| `docker volume rm myvolume` | Remove volume |
| `docker volume prune` | Remove unused volumes |
| `docker run -v myvolume:/data nginx` | Mount named volume |
| `docker run -v $(pwd):/app nginx` | Mount local directory |

---

# 5. Docker Network Commands

Networks connect containers together.

| Command | Explanation |
|---|---|
| `docker network ls` | List all networks |
| `docker network create mynetwork` | Create network |
| `docker network inspect mynetwork` | Show network details |
| `docker network rm mynetwork` | Remove network |
| `docker network connect mynetwork container` | Connect container to network |
| `docker network disconnect mynetwork container` | Disconnect container from network |
| `docker network prune` | Remove unused networks |

---

# 6. Docker Compose Commands

Docker Compose manages multi-container applications.

| Command | Explanation |
|---|---|
| `docker compose up` | Start services |
| `docker compose up -d` | Start services in background |
| `docker compose down` | Stop and remove services |
| `docker compose ps` | Show running services |
| `docker compose logs` | Show service logs |
| `docker compose logs -f` | Live logs |
| `docker compose build` | Build services |
| `docker compose pull` | Pull service images |
| `docker compose restart` | Restart services |
| `docker compose stop` | Stop services |
| `docker compose start` | Start stopped services |
| `docker compose exec web bash` | Run shell inside service |
| `docker compose config` | Validate compose file |
| `docker compose down -v` | Remove services with volumes |
| `docker compose pause` | Pause services |
| `docker compose unpause` | Resume services |
| `docker compose top` | Show service processes |
| `docker compose rm` | Remove stopped services |

---

# 7. Docker System Commands

| Command | Explanation |
|---|---|
| `docker system df` | Show Docker disk usage |
| `docker system prune` | Remove unused Docker resources |
| `docker system prune -a` | Remove all unused resources |
| `docker events` | Show Docker events in real time |

---

# 8. Docker Registry Commands

| Command | Explanation |
|---|---|
| `docker login` | Login to Docker Hub |
| `docker logout` | Logout from Docker Hub |
| `docker push username/image` | Upload image to registry |
| `docker pull username/private-image` | Pull private image |

---

# 9. Docker Swarm Commands

Docker Swarm provides container orchestration.

| Command | Explanation |
|---|---|
| `docker swarm init` | Initialize swarm cluster |
| `docker swarm join` | Join swarm cluster |
| `docker swarm leave` | Leave swarm |
| `docker node ls` | List swarm nodes |
| `docker service create nginx` | Create service |
| `docker service ls` | List services |
| `docker service scale web=5` | Scale service replicas |
| `docker service rm web` | Remove service |

---

# 10. Docker Buildx Commands

| Command | Explanation |
|---|---|
| `docker buildx ls` | List builders |
| `docker buildx create --name mybuilder` | Create builder |
| `docker buildx use mybuilder` | Use specific builder |
| `docker buildx inspect` | Show builder details |

---

# License

This project is open-source and free to use.

---

# Author

Shagor Robidas
Python & Django Developer
