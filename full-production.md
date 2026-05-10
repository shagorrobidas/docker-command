# 🐳 Django Docker Setup Guide (Production Ready)

![Python](https://img.shields.io/badge/Python-3.12-blue?style=for-the-badge&logo=python)
![Django](https://img.shields.io/badge/Django-5.x-green?style=for-the-badge&logo=django)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-336791?style=for-the-badge&logo=postgresql)
![Redis](https://img.shields.io/badge/Redis-7-DC382D?style=for-the-badge&logo=redis)
![Nginx](https://img.shields.io/badge/Nginx-Reverse%20Proxy-009639?style=for-the-badge&logo=nginx)
![Celery](https://img.shields.io/badge/Celery-Worker-37814A?style=for-the-badge&logo=celery)
![Grafana](https://img.shields.io/badge/Grafana-Monitoring-F46800?style=for-the-badge&logo=grafana)
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Production%20Ready-brightgreen?style=for-the-badge)

---

## 👨‍💻 Author

**Shagor Robidas**  
Senior Django & DevOps Engineer  
🔗 [GitHub](https://github.com/shagorrobidas) • 📧 roysagor88@gmail.com • 💼 [LinkedIn](https://linkedin.com/in/shagorrobidas)

---

## 📸 Screenshots

> _Add your application screenshots here._

```
screenshots/
├── dashboard.png
├── admin-panel.png
├── grafana-dashboard.png
└── api-docs.png
```

| Admin Panel | Grafana Dashboard |
|---|---|
| ![Admin](screenshots/admin-panel.png) | ![Grafana](screenshots/grafana-dashboard.png) |

---

## 📋 Project Overview

This is a **production-grade Django REST API** project with a complete DevOps infrastructure. It is built for scalability, observability, and security — ready to be deployed to any Linux server or cloud environment.

### What This Project Does

This project provides a full-stack backend platform (specifically a menopause health and wellness application — **Pearii**) featuring:

- 🤖 AI Chatbot with RAG (Retrieval-Augmented Generation) from PDF knowledge sources
- 📅 Daily health follow-up and tracking system
- 🥗 Recipe management
- 🏋️ Workout and exercise modules
- 📚 Resource/media library (Pearii Pedia)
- 👤 User authentication via Firebase
- 📊 REST API with pagination and permissions

### Why Docker?

Docker ensures that the application runs identically in **development**, **staging**, and **production** environments. It eliminates the classic "works on my machine" problem by packaging all dependencies, runtime configurations, and environment variables into isolated containers.

### Why This Setup Is Production-Ready

- **Gunicorn** handles concurrent requests efficiently (WSGI server)
- **Nginx** acts as a reverse proxy, serves static/media files, and provides SSL termination
- **PostgreSQL** is a battle-tested relational database for production workloads
- **Redis** handles caching and Celery message brokering
- **Celery + Beat** enables background tasks and scheduled jobs
- **Prometheus + Grafana** provides real-time observability and alerting
- **Docker Compose** orchestrates all services with health checks and restart policies
- **Environment variable isolation** keeps secrets out of source code

---

## 🛠 Tech Stack

| Category | Technology | Version |
|---|---|---|
| Language | Python | 3.12 |
| Framework | Django + DRF | 5.x |
| Containerization | Docker + Docker Compose | Latest |
| Database | PostgreSQL | 16 |
| Cache / Broker | Redis | 7 |
| Task Queue | Celery + Celery Beat | 5.x |
| Web Server | Nginx | 1.25 |
| WSGI Server | Gunicorn | 21.x |
| Monitoring | Prometheus + Grafana | Latest |
| Auth | Firebase Admin SDK | Latest |
| AI / RAG | ChromaDB + OpenAI / Claude | Latest |
| Storage | AWS S3 (optional) | - |
| CI/CD | GitHub Actions | - |

---

## 🏗 System Architecture

```
                          ┌─────────────────────────────────────────────────┐
                          │                  PRODUCTION SERVER               │
                          │                                                  │
  Internet Traffic        │   ┌──────────┐      ┌──────────────────────┐   │
  ─────────────────────── ▶  │  Nginx   │ ───▶ │   Gunicorn (WSGI)    │   │
  HTTPS :443 / HTTP :80   │   │ :80/:443 │      │   Django Application  │   │
                          │   └──────────┘      └──────────┬───────────┘   │
                          │        │                        │               │
                          │   Static/Media            ┌─────▼──────┐       │
                          │   Files Served            │  Database  │       │
                          │   Directly                │ PostgreSQL │       │
                          │                           │   :5432    │       │
                          │                           └────────────┘       │
                          │                                                  │
                          │   ┌────────────┐    ┌──────────────────────┐   │
                          │   │   Redis    │◀───│   Celery Worker(s)   │   │
                          │   │   :6379    │    │   (Async Tasks)      │   │
                          │   └─────┬──────┘    └──────────────────────┘   │
                          │         │                                        │
                          │   ┌─────▼──────┐    ┌──────────────────────┐   │
                          │   │ Celery Beat│    │  Prometheus + Grafana │   │
                          │   │ (Scheduler)│    │  (Monitoring :9090)  │   │
                          │   └────────────┘    └──────────────────────┘   │
                          │                                                  │
                          └─────────────────────────────────────────────────┘
```

---

## 📁 Project Structure

```
pearii-backend/
│
├── 📁 core/                        # Django project configuration
│   ├── settings/
│   │   ├── base.py                 # Shared settings
│   │   ├── development.py          # Dev overrides
│   │   └── production.py           # Prod overrides
│   ├── middleware/
│   │   └── language.py
│   ├── utils/
│   │   ├── exceptions.py
│   │   ├── media.py
│   │   └── response.py
│   ├── urls.py
│   ├── wsgi.py
│   └── asgi.py
│
├── 📁 users/                       # User management
├── 📁 ai_chatbot/                  # RAG chatbot engine
├── 📁 daily_follow_up/             # Health tracking
├── 📁 content_management/          # CMS
├── 📁 recipes/                     # Recipe module
├── 📁 resources/                   # Pearii Pedia / media library
├── 📁 workout/                     # Exercise & workouts
├── 📁 landing_page/                # Public landing page
├── 📁 api/                         # Shared API utilities
│
├── 📁 knowledge_source/            # RAG PDF documents
├── 📁 media/                       # Uploaded media files
├── 📁 static/                      # Collected static files
│
├── 📁 nginx/                       # Nginx config
│   ├── conf.d/default.conf
│   └── nginx.conf
│
├── 📄 Dockerfile                   # Production container
├── 📄 docker-compose.yml           # Full stack orchestration
├── 📄 entrypoint.sh                # Container startup script
├── 📄 requirements.txt             # Python dependencies
├── 📄 manage.py
├── 📄 .env                         # Environment variables (never commit)
├── 📄 .env.example                 # Sample env file (safe to commit)
├── 📄 .gitignore
└── 📄 README.md
```

---

## 🚀 Installation Guide

### Prerequisites

Make sure you have:
- Ubuntu 20.04 / 22.04 / 24.04 LTS
- At least 2 GB RAM and 20 GB disk space
- A non-root user with `sudo` privileges

---

### Step 1: Install Docker on Ubuntu

```bash
# Update system packages
sudo apt update && sudo apt upgrade -y

# Install required dependencies
sudo apt install -y ca-certificates curl gnupg lsb-release

# Add Docker's official GPG key
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add Docker repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine and Compose plugin
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io \
  docker-buildx-plugin docker-compose-plugin

# Enable and start Docker
sudo systemctl enable docker
sudo systemctl start docker

# Add current user to docker group (avoid sudo every time)
sudo usermod -aG docker $USER
newgrp docker

# Verify installation
docker --version
docker compose version
```

---

### Step 2: Clone & Setup the Project

```bash
# Clone the repository
git clone https://github.com/shagorrobidas/pearii-backend.git
cd pearii-backend

# Copy the example environment file
cp production.env.example .env

# Edit the .env file with your actual values
nano .env
```

---

### Step 3: Build and Run Containers

```bash
# Build all images (first time or after code changes)
docker compose build

# Start all services in detached mode
docker compose up -d

# Verify all containers are running
docker compose ps

# Run database migrations
docker compose exec web python manage.py migrate

# Collect static files
docker compose exec web python manage.py collectstatic --noinput

# Create a superuser for Django Admin
docker compose exec web python manage.py createsuperuser

# View logs in real time
docker compose logs -f web
```

---

## 🔐 Environment Variables

**Never commit your `.env` file to version control.** Use `.env.example` as a template.

### Sample `.env` File

```env
# ─────────────────────────────────────────────
# Django Core Settings
# ─────────────────────────────────────────────
SECRET_KEY=your-very-secure-random-secret-key-here
DEBUG=False
DJANGO_SETTINGS_MODULE=core.settings.production
ALLOWED_HOSTS=yourdomain.com,www.yourdomain.com,your-server-ip

# ─────────────────────────────────────────────
# Database (PostgreSQL)
# ─────────────────────────────────────────────
DB_ENGINE=django.db.backends.postgresql
DB_NAME=pearii_db
DB_USER=pearii_user
DB_PASSWORD=super_secure_db_password
DB_HOST=db
DB_PORT=5432

# ─────────────────────────────────────────────
# Redis
# ─────────────────────────────────────────────
REDIS_URL=redis://redis:6379/0
CELERY_BROKER_URL=redis://redis:6379/0
CELERY_RESULT_BACKEND=redis://redis:6379/0

# ─────────────────────────────────────────────
# AWS S3 (Optional Media Storage)
# ─────────────────────────────────────────────
USE_S3=True
AWS_ACCESS_KEY_ID=your-aws-access-key
AWS_SECRET_ACCESS_KEY=your-aws-secret-key
AWS_STORAGE_BUCKET_NAME=pearii-media-bucket
AWS_S3_REGION_NAME=us-east-1

# ─────────────────────────────────────────────
# Email Configuration
# ─────────────────────────────────────────────
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USE_TLS=True
EMAIL_HOST_USER=your-email@gmail.com
EMAIL_HOST_PASSWORD=your-app-password

# ─────────────────────────────────────────────
# Firebase
# ─────────────────────────────────────────────
FIREBASE_CREDENTIALS_PATH=/app/peariiapp-firebase-adminsdk.json

# ─────────────────────────────────────────────
# OpenAI / Anthropic (AI Chatbot)
# ─────────────────────────────────────────────
OPENAI_API_KEY=sk-your-openai-key
ANTHROPIC_API_KEY=sk-ant-your-anthropic-key

# ─────────────────────────────────────────────
# Monitoring
# ─────────────────────────────────────────────
GRAFANA_ADMIN_USER=admin
GRAFANA_ADMIN_PASSWORD=secure_grafana_password
```

> **Security tip:** Generate a strong `SECRET_KEY` with:
> ```bash
> python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
> ```

---

## 🐳 Docker Setup

### Dockerfile (Production)

```dockerfile
# ─────────────────────────────────────────────
# Stage 1: Builder
# ─────────────────────────────────────────────
FROM python:3.12-slim AS builder

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

WORKDIR /app

# Install system build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    libpq-dev \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Install Python dependencies into a virtual environment
COPY requirements.txt .
RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
RUN pip install --upgrade pip && pip install -r requirements.txt

# ─────────────────────────────────────────────
# Stage 2: Production Image
# ─────────────────────────────────────────────
FROM python:3.12-slim

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
ENV PATH="/opt/venv/bin:$PATH"

WORKDIR /app

# Only runtime dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    libpq-dev \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Copy venv from builder
COPY --from=builder /opt/venv /opt/venv

# Copy project files
COPY . .

# Copy and set entrypoint
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Create non-root user for security
RUN addgroup --system django && adduser --system --ingroup django django
RUN chown -R django:django /app
USER django

EXPOSE 8000

ENTRYPOINT ["/entrypoint.sh"]
```

### `entrypoint.sh`

```bash
#!/bin/sh
set -e

echo "Waiting for database..."
while ! nc -z "$DB_HOST" "$DB_PORT"; do
  sleep 0.5
done
echo "Database is ready."

echo "Running migrations..."
python manage.py migrate --noinput

echo "Collecting static files..."
python manage.py collectstatic --noinput

echo "Starting Gunicorn..."
exec gunicorn core.wsgi:application \
  --bind 0.0.0.0:8000 \
  --workers 4 \
  --worker-class gthread \
  --threads 2 \
  --timeout 120 \
  --access-logfile - \
  --error-logfile -
```

---

### `docker-compose.yml` (Full Stack)

```yaml
version: "3.9"

services:

  # ─────────────────────────────────────────
  # Django Application (Gunicorn)
  # ─────────────────────────────────────────
  web:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: pearii_web
    env_file: .env
    volumes:
      - static_volume:/app/staticfiles
      - media_volume:/app/media
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_healthy
    restart: unless-stopped
    networks:
      - pearii_network

  # ─────────────────────────────────────────
  # PostgreSQL Database
  # ─────────────────────────────────────────
  db:
    image: postgres:16-alpine
    container_name: pearii_db
    env_file: .env
    environment:
      POSTGRES_DB: ${DB_NAME}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER} -d ${DB_NAME}"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: unless-stopped
    networks:
      - pearii_network

  # ─────────────────────────────────────────
  # Redis (Cache + Celery Broker)
  # ─────────────────────────────────────────
  redis:
    image: redis:7-alpine
    container_name: pearii_redis
    command: redis-server --appendonly yes --maxmemory 256mb --maxmemory-policy allkeys-lru
    volumes:
      - redis_data:/data
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 3s
      retries: 3
    restart: unless-stopped
    networks:
      - pearii_network

  # ─────────────────────────────────────────
  # Celery Worker
  # ─────────────────────────────────────────
  celery_worker:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: pearii_celery_worker
    command: celery -A core worker --loglevel=info --concurrency=4
    env_file: .env
    depends_on:
      - redis
      - db
    restart: unless-stopped
    networks:
      - pearii_network

  # ─────────────────────────────────────────
  # Celery Beat (Scheduler)
  # ─────────────────────────────────────────
  celery_beat:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: pearii_celery_beat
    command: celery -A core beat --loglevel=info --scheduler django_celery_beat.schedulers:DatabaseScheduler
    env_file: .env
    depends_on:
      - redis
      - db
    restart: unless-stopped
    networks:
      - pearii_network

  # ─────────────────────────────────────────
  # Nginx (Reverse Proxy)
  # ─────────────────────────────────────────
  nginx:
    image: nginx:1.25-alpine
    container_name: pearii_nginx
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/conf.d:/etc/nginx/conf.d
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
      - static_volume:/app/staticfiles
      - media_volume:/app/media
      - ./nginx/ssl:/etc/nginx/ssl        # SSL certificates
    depends_on:
      - web
    restart: unless-stopped
    networks:
      - pearii_network

  # ─────────────────────────────────────────
  # Prometheus
  # ─────────────────────────────────────────
  prometheus:
    image: prom/prometheus:latest
    container_name: pearii_prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./monitoring/prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus_data:/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.retention.time=30d'
    restart: unless-stopped
    networks:
      - pearii_network

  # ─────────────────────────────────────────
  # Grafana
  # ─────────────────────────────────────────
  grafana:
    image: grafana/grafana:latest
    container_name: pearii_grafana
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_USER=${GRAFANA_ADMIN_USER}
      - GF_SECURITY_ADMIN_PASSWORD=${GRAFANA_ADMIN_PASSWORD}
    volumes:
      - grafana_data:/var/lib/grafana
      - ./monitoring/grafana/dashboards:/etc/grafana/provisioning/dashboards
    depends_on:
      - prometheus
    restart: unless-stopped
    networks:
      - pearii_network

# ─────────────────────────────────────────
# Volumes
# ─────────────────────────────────────────
volumes:
  postgres_data:
  redis_data:
  static_volume:
  media_volume:
  prometheus_data:
  grafana_data:

# ─────────────────────────────────────────
# Network
# ─────────────────────────────────────────
networks:
  pearii_network:
    driver: bridge
```

---

## 🔧 Services Explanation

### 🌐 Web (Django + Gunicorn)

The core application server. **Gunicorn** is a production-grade WSGI server that spawns multiple worker processes to handle concurrent HTTP requests. The Django app is never exposed directly to the internet — all traffic passes through Nginx first.

### 🗄 Database (PostgreSQL)

Stores all relational data: users, health records, recipes, workouts, and more. Uses Docker volumes for persistent storage. The `healthcheck` ensures the web service only starts after the database is ready to accept connections.

### ⚡ Redis

Serves dual roles: (1) **Cache** — stores frequently accessed data (API responses, sessions) to reduce database load, and (2) **Message Broker** — queues background tasks for Celery workers to consume.

### 🔄 Celery Worker

Processes background tasks asynchronously so they don't block the web request-response cycle. Examples: sending emails, generating PDFs, running AI inference, syncing data.

### ⏰ Celery Beat

A built-in scheduler that triggers periodic tasks (like nightly health reports, digest emails, data cleanup) on a defined schedule. Uses `django-celery-beat` to manage schedules via the Django Admin.

### 🔀 Nginx

Acts as the **reverse proxy** sitting in front of Gunicorn. Responsibilities include: SSL termination, serving static and media files directly (bypassing Django for performance), rate limiting, and load balancing.

### 📈 Prometheus

Scrapes metrics from Django (via `django-prometheus`), Nginx, Redis, and PostgreSQL exporters at regular intervals. Stores time-series data for querying.

### 📊 Grafana

Visualizes Prometheus metrics through beautiful, interactive dashboards. Provides real-time monitoring, alerting, and historical analysis of application performance and infrastructure health.

---

## 🌐 Nginx Configuration

### `nginx/conf.d/default.conf`

```nginx
upstream django_app {
    server web:8000;
}

# ─────────────────────────────────────────
# HTTP → HTTPS Redirect
# ─────────────────────────────────────────
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }

    location / {
        return 301 https://$host$request_uri;
    }
}

# ─────────────────────────────────────────
# HTTPS Server Block
# ─────────────────────────────────────────
server {
    listen 443 ssl http2;
    server_name yourdomain.com www.yourdomain.com;

    # SSL Certificates (Let's Encrypt)
    ssl_certificate     /etc/nginx/ssl/fullchain.pem;
    ssl_certificate_key /etc/nginx/ssl/privkey.pem;
    ssl_protocols       TLSv1.2 TLSv1.3;
    ssl_ciphers         HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;
    ssl_session_cache   shared:SSL:10m;

    # Security Headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;
    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload" always;

    # File upload size limit
    client_max_body_size 50M;

    # ─────────────────────────────────────
    # Static Files (served directly)
    # ─────────────────────────────────────
    location /static/ {
        alias /app/staticfiles/;
        expires 30d;
        access_log off;
        add_header Cache-Control "public, max-age=2592000";
    }

    # ─────────────────────────────────────
    # Media Files (served directly)
    # ─────────────────────────────────────
    location /media/ {
        alias /app/media/;
        expires 7d;
        access_log off;
    }

    # ─────────────────────────────────────
    # Django Application (proxied)
    # ─────────────────────────────────────
    location / {
        proxy_pass          http://django_app;
        proxy_set_header    Host $host;
        proxy_set_header    X-Real-IP $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header    X-Forwarded-Proto $scheme;
        proxy_read_timeout  120s;
        proxy_connect_timeout 10s;
    }
}
```

> **SSL Setup with Let's Encrypt (Certbot):**
> ```bash
> sudo apt install certbot
> sudo certbot certonly --standalone -d yourdomain.com -d www.yourdomain.com
> # Certs saved to: /etc/letsencrypt/live/yourdomain.com/
> ```

---

## 🔁 CI/CD Pipeline

### `.github/workflows/deploy.yml`

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

env:
  DOCKER_IMAGE: ghcr.io/${{ github.repository_owner }}/pearii-backend

jobs:

  # ─────────────────────────────────────────
  # Job 1: Run Tests
  # ─────────────────────────────────────────
  test:
    name: Run Django Tests
    runs-on: ubuntu-latest

    services:
      postgres:
        image: postgres:16
        env:
          POSTGRES_DB: test_db
          POSTGRES_USER: test_user
          POSTGRES_PASSWORD: test_pass
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

      redis:
        image: redis:7-alpine
        options: >-
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.12"
          cache: "pip"

      - name: Install Dependencies
        run: pip install -r requirements.txt

      - name: Run Tests
        env:
          DJANGO_SETTINGS_MODULE: core.settings.development
          DB_HOST: localhost
          DB_NAME: test_db
          DB_USER: test_user
          DB_PASSWORD: test_pass
          REDIS_URL: redis://localhost:6379/0
          SECRET_KEY: test-secret-key-for-ci
        run: python manage.py test --verbosity=2

  # ─────────────────────────────────────────
  # Job 2: Build & Push Docker Image
  # ─────────────────────────────────────────
  build:
    name: Build Docker Image
    runs-on: ubuntu-latest
    needs: test
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Log in to GitHub Container Registry
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Build and Push Image
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: |
            ${{ env.DOCKER_IMAGE }}:latest
            ${{ env.DOCKER_IMAGE }}:${{ github.sha }}
          cache-from: type=gha
          cache-to: type=gha,mode=max

  # ─────────────────────────────────────────
  # Job 3: Deploy to Production
  # ─────────────────────────────────────────
  deploy:
    name: Deploy to Server
    runs-on: ubuntu-latest
    needs: build
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Deploy via SSH
        uses: appleboy/ssh-action@v1.0.0
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.SERVER_SSH_KEY }}
          script: |
            cd /opt/pearii-backend
            git pull origin main
            docker compose pull
            docker compose up -d --no-deps --build web celery_worker celery_beat
            docker compose exec -T web python manage.py migrate --noinput
            docker compose exec -T web python manage.py collectstatic --noinput
            docker image prune -f
            echo "✅ Deployment complete!"
```

> **Required GitHub Secrets:**
> - `SERVER_HOST` — your server IP or domain
> - `SERVER_USER` — SSH username (e.g., `ubuntu`)
> - `SERVER_SSH_KEY` — private SSH key for server access

---

## 🗃 Database Migrations & Commands

All Django management commands should be run **inside the `web` container**:

```bash
# Run migrations
docker compose exec web python manage.py migrate

# Create a new migration
docker compose exec web python manage.py makemigrations <app_name>

# Show migration status
docker compose exec web python manage.py showmigrations

# Create superuser
docker compose exec web python manage.py createsuperuser

# Open Django shell
docker compose exec web python manage.py shell

# Collect static files
docker compose exec web python manage.py collectstatic --noinput

# Check for deployment issues
docker compose exec web python manage.py check --deploy

# Flush database (DANGER: deletes all data)
docker compose exec web python manage.py flush

# Access PostgreSQL directly
docker compose exec db psql -U pearii_user -d pearii_db

# Access Redis CLI
docker compose exec redis redis-cli

# View Celery worker status
docker compose exec celery_worker celery -A core inspect active

# Restart only the web service (zero-downtime-ish)
docker compose up -d --no-deps --build web
```

---

## 📊 Monitoring

### Prometheus Setup

Create `monitoring/prometheus.yml`:

```yaml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: "django"
    static_configs:
      - targets: ["web:8000"]
    metrics_path: /metrics

  - job_name: "redis"
    static_configs:
      - targets: ["redis-exporter:9121"]

  - job_name: "postgres"
    static_configs:
      - targets: ["postgres-exporter:9187"]

  - job_name: "nginx"
    static_configs:
      - targets: ["nginx-exporter:9113"]
```

Install `django-prometheus` to expose metrics:

```bash
pip install django-prometheus
```

```python
# settings/base.py
INSTALLED_APPS = [
    ...
    'django_prometheus',
]

MIDDLEWARE = [
    'django_prometheus.middleware.PrometheusBeforeMiddleware',
    ...
    'django_prometheus.middleware.PrometheusAfterMiddleware',
]
```

```python
# core/urls.py
urlpatterns = [
    ...
    path('', include('django_prometheus.urls')),
]
```

### Grafana Dashboard Setup

1. Open Grafana at `http://yourdomain.com:3000`
2. Login with credentials from `.env`
3. Go to **Configuration → Data Sources → Add data source**
4. Select **Prometheus** → set URL to `http://prometheus:9090`
5. Click **Save & Test**
6. Go to **Dashboards → Import**
7. Import dashboard ID `17658` (Django + Gunicorn) or upload your own JSON

**Key metrics to monitor:**

| Metric | Meaning |
|---|---|
| `django_http_requests_total` | Total HTTP requests |
| `django_http_request_duration_seconds` | Response time distribution |
| `django_db_execute_total` | Database query count |
| `redis_connected_clients` | Redis active connections |
| `process_resident_memory_bytes` | Memory consumption |

---

## 🚀 Advanced Production Features

### ⚡ Auto-Scaling with Kubernetes

While Docker Compose is ideal for single-server deployments, **Kubernetes (K8s)** enables horizontal auto-scaling across a cluster. Key concepts:

- **Deployment**: Manages pod replicas of your Django container
- **HorizontalPodAutoscaler (HPA)**: Automatically scales pods based on CPU/memory usage
- **Service**: Load balances traffic across pods
- **Ingress**: Replaces Nginx at the cluster level (often uses Nginx Ingress Controller)

```yaml
# k8s/hpa.yaml (example)
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: django-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: django-web
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
```

For managed K8s, use **Amazon EKS**, **Google GKE**, or **DigitalOcean Kubernetes**.

---

### ☁️ AWS S3 Media Storage

Store user-uploaded media files on **Amazon S3** instead of local disk — essential for multi-server or Kubernetes deployments where local volumes aren't shared.

```bash
pip install django-storages boto3
```

```python
# settings/production.py
if env.bool("USE_S3", default=False):
    DEFAULT_FILE_STORAGE = "storages.backends.s3boto3.S3Boto3Storage"
    STATICFILES_STORAGE = "storages.backends.s3boto3.S3StaticStorage"

    AWS_ACCESS_KEY_ID = env("AWS_ACCESS_KEY_ID")
    AWS_SECRET_ACCESS_KEY = env("AWS_SECRET_ACCESS_KEY")
    AWS_STORAGE_BUCKET_NAME = env("AWS_STORAGE_BUCKET_NAME")
    AWS_S3_REGION_NAME = env("AWS_S3_REGION_NAME", default="us-east-1")
    AWS_S3_CUSTOM_DOMAIN = f"{AWS_STORAGE_BUCKET_NAME}.s3.amazonaws.com"
    AWS_DEFAULT_ACL = "public-read"
    AWS_S3_OBJECT_PARAMETERS = {"CacheControl": "max-age=86400"}

    MEDIA_URL = f"https://{AWS_S3_CUSTOM_DOMAIN}/media/"
    STATIC_URL = f"https://{AWS_S3_CUSTOM_DOMAIN}/static/"
```

> **S3 Bucket Policy:** Make the `media/` prefix publicly readable, keep `static/` public, and block direct PUT from the internet.

---

### 📊 ELK Stack Logging

The **ELK Stack** (Elasticsearch, Logstash, Kibana) provides centralized, searchable log aggregation.

```yaml
# Add to docker-compose.yml
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:8.12.0
    environment:
      - discovery.type=single-node
      - ES_JAVA_OPTS=-Xms512m -Xmx512m
    volumes:
      - es_data:/usr/share/elasticsearch/data
    networks:
      - pearii_network

  logstash:
    image: docker.elastic.co/logstash/logstash:8.12.0
    volumes:
      - ./monitoring/logstash/pipeline:/usr/share/logstash/pipeline
    depends_on:
      - elasticsearch
    networks:
      - pearii_network

  kibana:
    image: docker.elastic.co/kibana/kibana:8.12.0
    ports:
      - "5601:5601"
    environment:
      ELASTICSEARCH_URL: http://elasticsearch:9200
    depends_on:
      - elasticsearch
    networks:
      - pearii_network
```

Configure Django to ship logs to Logstash via **python-logstash** or **structlog**:

```python
# settings/production.py
LOGGING = {
    "version": 1,
    "handlers": {
        "logstash": {
            "class": "logstash.TCPLogstashHandler",
            "host": "logstash",
            "port": 5959,
            "version": 1,
        },
    },
    "root": {"handlers": ["logstash"], "level": "INFO"},
}
```

---

### 🔐 Production Security Checklist

#### Django Security

```python
# settings/production.py
DEBUG = False
SECRET_KEY = env("SECRET_KEY")                    # Never hardcode
ALLOWED_HOSTS = env.list("ALLOWED_HOSTS")

# HTTPS enforcement
SECURE_SSL_REDIRECT = True
SECURE_HSTS_SECONDS = 63072000
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
SECURE_HSTS_PRELOAD = True

# Cookie security
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
SESSION_COOKIE_HTTPONLY = True

# Content security
X_FRAME_OPTIONS = "DENY"
SECURE_CONTENT_TYPE_NOSNIFF = True
SECURE_BROWSER_XSS_FILTER = True
```

#### Docker Security

- Run containers as **non-root users** (see Dockerfile `USER django`)
- Use **read-only volumes** where possible
- Never expose database or Redis ports to the public internet
- Use **Docker secrets** or a vault (e.g., HashiCorp Vault, AWS Secrets Manager) for sensitive values
- Regularly update base images and scan with `docker scout` or `trivy`

#### Server Security

```bash
# UFW Firewall — only allow required ports
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp     # SSH
sudo ufw allow 80/tcp     # HTTP
sudo ufw allow 443/tcp    # HTTPS
sudo ufw enable

# Disable root SSH login
sudo sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
sudo systemctl restart ssh

# Install and configure fail2ban
sudo apt install fail2ban -y
sudo systemctl enable fail2ban

# Automatic security updates
sudo apt install unattended-upgrades -y
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

---

## ✅ Best Practices

### 🔒 Environment & Secrets

- Always use `.env` files, never hardcode secrets in code
- Add `.env` to `.gitignore` immediately
- Use `.env.example` as a documented template for onboarding
- Rotate secrets regularly; use a secret manager in large teams

### 🛡 Security

- Run `python manage.py check --deploy` before every production release
- Keep `DEBUG=False` in production at all times
- Use HTTPS everywhere; redirect HTTP to HTTPS via Nginx
- Implement rate limiting on authentication endpoints
- Regularly audit `INSTALLED_APPS` and `MIDDLEWARE`

### ⚡ Performance

- Use `select_related()` and `prefetch_related()` to minimize N+1 queries
- Cache expensive API responses with Redis (`django-cache-machine` or DRF caching)
- Use Gunicorn with `gthread` workers for I/O-bound workloads
- Enable PostgreSQL connection pooling with **PgBouncer** at scale
- Serve static/media files via Nginx or a CDN (CloudFront, Cloudflare)

### 🚢 Production Deployment Rules

- Always run migrations before deploying new code: `python manage.py migrate`
- Use `--no-deps --build` to rebuild only changed services
- Never run `manage.py runserver` in production — always Gunicorn
- Keep container logs centralized (ELK or CloudWatch)
- Set up health check endpoints (`/health/`) and monitor them with uptime tools
- Automate database backups:
  ```bash
  # Daily PostgreSQL backup to S3
  docker compose exec db pg_dump -U $DB_USER $DB_NAME | \
    gzip | aws s3 cp - s3://your-bucket/backups/$(date +%F).sql.gz
  ```
- Tag Docker images with git SHA for traceability and rollback capability

---

## 🧪 Useful Commands Quick Reference

```bash
# ─── Container Management ───────────────────────────────
docker compose up -d                        # Start all services
docker compose down                         # Stop all services
docker compose down -v                      # Stop + delete volumes (CAREFUL!)
docker compose restart web                  # Restart a single service
docker compose logs -f web                  # Follow logs
docker compose ps                           # Check service status

# ─── Django ─────────────────────────────────────────────
docker compose exec web python manage.py migrate
docker compose exec web python manage.py createsuperuser
docker compose exec web python manage.py shell
docker compose exec web python manage.py check --deploy

# ─── Database ───────────────────────────────────────────
docker compose exec db pg_dump -U pearii_user pearii_db > backup.sql
docker compose exec -T db psql -U pearii_user pearii_db < backup.sql

# ─── Celery ─────────────────────────────────────────────
docker compose exec celery_worker celery -A core inspect active
docker compose exec celery_worker celery -A core inspect stats

# ─── Cleanup ────────────────────────────────────────────
docker system prune -af                     # Remove unused images/containers
docker volume prune                         # Remove unused volumes
```

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgements

- [Django Documentation](https://docs.djangoproject.com/)
- [Docker Official Docs](https://docs.docker.com/)
- [Celery Documentation](https://docs.celeryq.dev/)
- [Prometheus + Grafana](https://grafana.com/docs/)
- [django-prometheus](https://github.com/korfuri/django-prometheus)
- [django-storages](https://django-storages.readthedocs.io/)

---

<div align="center">

**Built with ❤️ by [Shagor Robidas](https://github.com/shagorrobidas)**

⭐ Star this repo if it helped you! ⭐

</div>
