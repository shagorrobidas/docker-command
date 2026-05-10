# Django Docker Setup Guide

Author: Shagor Robidas

A complete beginner-friendly guide to Dockerize a Django project using Docker and Docker Compose.

---

# Project Overview

This project demonstrates how to:

- Install Docker
- Create a Django project
- Dockerize Django
- Use Docker Compose
- Configure PostgreSQL
- Run Django in containers
- Use production-ready setup with Gunicorn

---

# Tech Stack

- Python 3
- Django
- Docker
- Docker Compose
- PostgreSQL
- Gunicorn

---

# Project Structure

```text
django-docker-app/
│
├── config/
├── app/
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── .dockerignore
├── manage.py
└── .env
```

---

# 1. Install Docker

## Ubuntu Installation

Update system:

```bash
sudo apt update
sudo apt upgrade -y
```

Install Docker:

```bash
sudo apt install docker.io docker-compose-v2 -y
```

Start Docker:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

Run Docker without sudo:

```bash
sudo usermod -aG docker $USER
newgrp docker
```

Check installation:

```bash
docker --version
docker compose version
```

---

# 2. Create Django Project

Create project folder:

```bash
mkdir django-docker-app
cd django-docker-app
```

Create virtual environment:

```bash
python3 -m venv venv
```

Activate environment:

```bash
source venv/bin/activate
```

Install Django:

```bash
pip install django
```

Create Django project:

```bash
django-admin startproject config .
```

Run development server:

```bash
python manage.py runserver
```

Open browser:

```text
http://127.0.0.1:8000
```

---

# 3. Create requirements.txt

Save dependencies:

```bash
pip freeze > requirements.txt
```

Example:

```txt
Django>=5.0
gunicorn
psycopg2-binary
```

---

# 4. Create Dockerfile

Create a file named:

```text
Dockerfile
```

Add:

```Dockerfile
FROM python:3.12-slim

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

WORKDIR /app

COPY requirements.txt .

RUN pip install --upgrade pip
RUN pip install -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
```

---

# Dockerfile Explanation

| Command | Description |
|---|---|
| `FROM` | Base image |
| `WORKDIR` | Set working directory |
| `COPY` | Copy files |
| `RUN` | Execute commands |
| `EXPOSE` | Open container port |
| `CMD` | Default startup command |

---

# 5. Create .dockerignore

Create:

```text
.dockerignore
```

Add:

```text
venv/
__pycache__/
*.pyc
db.sqlite3
.env
.git
```

---

# 6. Create docker-compose.yml

Create:

```text
docker-compose.yml
```

Add:

```yaml
services:
  web:
    build: .
    container_name: django_app
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/app
    ports:
      - "8000:8000"
```

---

# Docker Compose Explanation

| Option | Description |
|---|---|
| `build` | Build Docker image |
| `container_name` | Set custom container name |
| `command` | Run startup command |
| `volumes` | Sync local files |
| `ports` | Map container ports |

---

# 7. Build Docker Container

Build image:

```bash
docker compose build
```

---

# 8. Run Django with Docker

Start container:

```bash
docker compose up
```

Run in background:

```bash
docker compose up -d
```

Check running containers:

```bash
docker ps
```

Open browser:

```text
http://localhost:8000
```

---

# 9. Run Django Commands Inside Docker

Make migrations:

```bash
docker compose exec web python manage.py makemigrations
```

Apply migrations:

```bash
docker compose exec web python manage.py migrate
```

Create superuser:

```bash
docker compose exec web python manage.py createsuperuser
```

---

# 10. Stop Docker Container

```bash
docker compose down
```

---

# 11. Rebuild Docker Container

```bash
docker compose up --build
```

---

# 12. Add PostgreSQL Database

Update `docker-compose.yml`:

```yaml
services:
  db:
    image: postgres:16
    container_name: postgres_db
    environment:
      POSTGRES_DB: django_db
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    ports:
      - "5432:5432"

  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/app
    ports:
      - "8000:8000"
    depends_on:
      - db
```

---

# Install PostgreSQL Driver

Add in `requirements.txt`:

```txt
psycopg2-binary
```

Rebuild image:

```bash
docker compose build
```

---

# Configure Django Database

Edit:

```text
config/settings.py
```

Replace database configuration:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'django_db',
        'USER': 'postgres',
        'PASSWORD': 'postgres',
        'HOST': 'db',
        'PORT': 5432,
    }
}
```

Run migrations:

```bash
docker compose exec web python manage.py migrate
```

---

# 13. Static Files Setup

Add in `settings.py`:

```python
STATIC_URL = 'static/'
STATIC_ROOT = BASE_DIR / "staticfiles"
```

Collect static files:

```bash
docker compose exec web python manage.py collectstatic
```

---

# 14. Media Files Setup

Add:

```python
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'
```

---

# 15. Production Setup

Install Gunicorn:

```txt
gunicorn
```

Update Dockerfile:

```Dockerfile
CMD ["gunicorn", "config.wsgi:application", "--bind", "0.0.0.0:8000"]
```

Build production container:

```bash
docker compose up --build
```

---

# 16. Useful Docker Commands

| Command | Description |
|---|---|
| `docker ps` | Show running containers |
| `docker images` | Show Docker images |
| `docker logs container` | Show container logs |
| `docker compose logs -f` | Live logs |
| `docker compose exec web bash` | Open terminal |
| `docker compose restart` | Restart services |
| `docker compose down` | Stop containers |
| `docker system prune -a` | Remove unused resources |

---

# 17. Common Problems & Solutions

## Permission Denied

Fix:

```bash
sudo usermod -aG docker $USER
newgrp docker
```

---

## Port Already Used

Change port:

```yaml
ports:
  - "8001:8000"
```

---

## Container Not Starting

Check logs:

```bash
docker compose logs
```

---

## Database Connection Error

Check:

- PostgreSQL container running
- Database settings correct
- `depends_on` added

---

# 18. Best Practices

- Use `.env` file for secrets
- Use PostgreSQL in production
- Use Gunicorn + Nginx
- Keep Docker images lightweight
- Use `.dockerignore`
- Use named volumes for databases

---

# 19. Docker Workflow

Build image:

```bash
docker compose build
```

Start container:

```bash
docker compose up -d
```

Run migrations:

```bash
docker compose exec web python manage.py migrate
```

Create superuser:

```bash
docker compose exec web python manage.py createsuperuser
```

View logs:

```bash
docker compose logs -f
```

Stop services:

```bash
docker compose down
```

---

# 20. Official Documentation

Docker Docs:
https://docs.docker.com/

Django Docs:
https://docs.djangoproject.com/

Docker Hub:
https://hub.docker.com/

---

# License

This project is open-source and free to use.

---

# Author

Shagor Robidas  
Python & Django Developer

---
