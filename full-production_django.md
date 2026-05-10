# 🚀 AWS Full Production Architecture — Django + Docker

![Python](https://img.shields.io/badge/Python-3.12-blue?style=for-the-badge&logo=python)
![Django](https://img.shields.io/badge/Django-5.x-green?style=for-the-badge&logo=django)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker)
![AWS EC2](https://img.shields.io/badge/AWS-EC2-FF9900?style=for-the-badge&logo=amazonec2)
![AWS RDS](https://img.shields.io/badge/AWS-RDS-527FFF?style=for-the-badge&logo=amazonrds)
![AWS S3](https://img.shields.io/badge/AWS-S3-569A31?style=for-the-badge&logo=amazons3)
![AWS ALB](https://img.shields.io/badge/AWS-Load%20Balancer-FF9900?style=for-the-badge&logo=awselasticloadbalancing)
![Nginx](https://img.shields.io/badge/Nginx-Reverse%20Proxy-009639?style=for-the-badge&logo=nginx)
![Celery](https://img.shields.io/badge/Celery-Worker-37814A?style=for-the-badge&logo=celery)
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Production%20Ready-brightgreen?style=for-the-badge)

---

## 👨‍💻 Author

**Shagor Robidas**
Senior Django & DevOps Engineer
🔗 [GitHub](https://github.com/shagorrobidas) • 📧 shagorrobidas@email.com • 💼 [LinkedIn](https://linkedin.com/in/shagorrobidas)

---

## 📋 Project Overview

This guide covers a **complete AWS production deployment** for a Django application using Docker Compose on EC2. Instead of running PostgreSQL and Redis as containers, **AWS managed services** (RDS + ElastiCache) replace them — giving you automatic backups, Multi-AZ failover, and zero maintenance overhead.

### Architecture at a Glance

```
Internet → Route 53 (DNS) → ALB (HTTPS/SSL) → EC2 (Docker Compose)
                                                    ├── Nginx container
                                                    ├── Gunicorn + Django container
                                                    ├── Celery Worker container
                                                    └── Celery Beat container
                                                         ↓              ↓             ↓
                                                    RDS PostgreSQL  ElastiCache S3 + CloudFront
                                                    (managed)       Redis        (files)
```

### What Runs Where

| Component | Location |
|---|---|
| Nginx | Docker container on EC2 |
| Gunicorn + Django | Docker container on EC2 |
| Celery Worker | Docker container on EC2 |
| Celery Beat | Docker container on EC2 |
| PostgreSQL | AWS RDS (fully managed) |
| Redis | AWS ElastiCache (fully managed) |
| Static & Media Files | AWS S3 + CloudFront CDN |
| Docker Images | AWS ECR (Elastic Container Registry) |
| SSL Certificate | AWS ACM on ALB |
| DNS Routing | AWS Route 53 |

---

## 🛠 Tech Stack

| Category | Technology |
|---|---|
| Language | Python 3.12 |
| Framework | Django 5.x + DRF |
| Containerization | Docker + Docker Compose |
| Web Server | Nginx 1.25 |
| WSGI Server | Gunicorn |
| Task Queue | Celery + Celery Beat |
| Database | AWS RDS PostgreSQL 16 |
| Cache / Broker | AWS ElastiCache Redis 7 |
| File Storage | AWS S3 + CloudFront |
| Image Registry | AWS ECR |
| Load Balancer | AWS Application Load Balancer |
| SSL | AWS Certificate Manager (ACM) |
| DNS | AWS Route 53 |
| CI/CD | GitHub Actions |

---

## 🏗 System Architecture (ASCII)

```
                        ┌─────────────────────────────────────────────┐
                        │              AWS CLOUD                       │
                        │                                              │
  Users / Internet      │   ┌──────────┐     ┌────────────────────┐  │
  ─────────────────────▶│   │ Route 53 │────▶│  App Load Balancer │  │
  HTTPS requests         │   │   DNS    │     │  HTTPS :443        │  │
                        │   └──────────┘     │  ACM SSL cert      │  │
                        │                    └─────────┬──────────┘  │
                        │                              │              │
                        │              ┌───────────────▼────────────┐ │
                        │              │  EC2 Instance (t3.medium)  │ │
                        │              │  ┌──────────────────────┐  │ │
                        │              │  │   Docker Compose     │  │ │
                        │              │  │                      │  │ │
                        │              │  │  [Nginx :80]         │  │ │
                        │              │  │      ↓               │  │ │
                        │              │  │  [Gunicorn :8000]    │  │ │
                        │              │  │  [Django App]        │  │ │
                        │              │  │      ↓               │  │ │
                        │              │  │  [Celery Worker]     │  │ │
                        │              │  │  [Celery Beat]       │  │ │
                        │              │  └──────────────────────┘  │ │
                        │              └───────────────┬────────────┘ │
                        │                    ┌─────────┼──────────┐   │
                        │                    ▼         ▼          ▼   │
                        │              ┌─────────┐ ┌──────┐ ┌──────┐ │
                        │              │   RDS   │ │Redis │ │  S3  │ │
                        │              │Postgres │ │Cache │ │+CDN  │ │
                        │              └─────────┘ └──────┘ └──────┘ │
                        │                                              │
                        │   ┌──────────────────────────────────────┐  │
                        │   │ ECR — Docker Image Registry          │  │
                        │   └──────────────────────────────────────┘  │
                        └─────────────────────────────────────────────┘
```

---

## 📁 Project Structure

```
pearii-backend/
│
├── 📁 core/                        # Django project config
│   ├── settings/
│   │   ├── base.py
│   │   ├── development.py
│   │   └── production.py           # AWS-specific settings
│   ├── urls.py
│   ├── wsgi.py
│   └── asgi.py
│
├── 📁 users/
├── 📁 ai_chatbot/
├── 📁 daily_follow_up/
├── 📁 content_management/
├── 📁 recipes/
├── 📁 resources/
├── 📁 workout/
├── 📁 landing_page/
├── 📁 api/
│
├── 📁 nginx/
│   └── conf.d/default.conf         # Nginx reverse proxy config
│
├── 📁 .github/
│   └── workflows/
│       └── deploy.yml              # CI/CD pipeline
│
├── 📄 Dockerfile                   # Multi-stage production build
├── 📄 docker-compose.prod.yml      # AWS production compose (no db/redis)
├── 📄 docker-compose.yml           # Local dev compose (with db/redis)
├── 📄 entrypoint.sh
├── 📄 requirements.txt
├── 📄 .env                         # Never commit — real secrets
├── 📄 .env.example                 # Safe template to commit
└── 📄 README.md
```

---

## 🔧 Prerequisites

Before starting, make sure you have:

- AWS account with billing enabled
- AWS CLI installed and configured
- Docker + Docker Compose installed locally
- Domain name (for Route 53 + SSL)
- GitHub repository for CI/CD

```bash
# Install AWS CLI
pip install awscli

# Configure AWS CLI
aws configure
# Prompts: Access Key, Secret Key, Region (us-east-1), output (json)

# Verify
aws sts get-caller-identity
```

---

## 🌐 Phase 1 — VPC & Networking

### Step 1.1 — Create VPC

```
AWS Console → VPC → Create VPC
  Name:      pearii-vpc
  IPv4 CIDR: 10.0.0.0/16
```

### Step 1.2 — Create Subnets

Create four subnets across two Availability Zones:

| Name | AZ | CIDR Block | Type |
|---|---|---|---|
| pearii-public-1a | us-east-1a | 10.0.1.0/24 | Public |
| pearii-public-1b | us-east-1b | 10.0.2.0/24 | Public |
| pearii-private-1a | us-east-1a | 10.0.3.0/24 | Private |
| pearii-private-1b | us-east-1b | 10.0.4.0/24 | Private |

### Step 1.3 — Internet Gateway & NAT

```
# Internet Gateway
VPC → Internet Gateways → Create → Attach to pearii-vpc

# Public Route Table
  Route: 0.0.0.0/0 → Internet Gateway
  Associate: pearii-public-1a, pearii-public-1b

# NAT Gateway (create in public subnet)
VPC → NAT Gateways → Create → select pearii-public-1a → Allocate Elastic IP

# Private Route Table
  Route: 0.0.0.0/0 → NAT Gateway
  Associate: pearii-private-1a, pearii-private-1b
```

### Step 1.4 — Security Groups

Create these four security groups inside `pearii-vpc`:

```
# 1. ALB Security Group: pearii-alb-sg
Inbound:
  HTTP  80   Source: 0.0.0.0/0
  HTTPS 443  Source: 0.0.0.0/0

# 2. EC2 Security Group: pearii-ec2-sg
Inbound:
  HTTP  80  Source: pearii-alb-sg   (only from load balancer)
  SSH   22  Source: Your IP/32

# 3. RDS Security Group: pearii-rds-sg
Inbound:
  PostgreSQL 5432  Source: pearii-ec2-sg

# 4. Redis Security Group: pearii-redis-sg
Inbound:
  Custom TCP 6379  Source: pearii-ec2-sg
```

---

## 🗄 Phase 2 — RDS PostgreSQL Setup

```
AWS Console → RDS → Create Database

  Engine:           PostgreSQL 16
  Template:         Production
  DB Identifier:    pearii-db
  Master Username:  pearii_user
  Master Password:  <strong-password>
  Instance class:   db.t3.medium
  Storage:          100 GB gp3, autoscaling enabled
  Multi-AZ:         Yes
  VPC:              pearii-vpc
  Subnet group:     New → private subnets only
  Security group:   pearii-rds-sg
  Public access:    No
  Database name:    pearii_db
  Backup retention: 7 days
```

After creation, save your **RDS endpoint**:
```
pearii-db.xxxxxxxxx.us-east-1.rds.amazonaws.com
```

---

## ⚡ Phase 3 — ElastiCache Redis Setup

```
AWS Console → ElastiCache → Create cluster

  Cluster mode:     Disabled
  Name:             pearii-redis
  Engine:           Redis 7.x
  Node type:        cache.t3.medium
  Replicas:         1
  VPC:              pearii-vpc
  Subnet group:     New → private subnets
  Security group:   pearii-redis-sg
```

After creation, save your **Primary endpoint**:
```
pearii-redis.xxxxxx.cache.amazonaws.com:6379
```

---

## ☁️ Phase 4 — S3 Bucket Setup

### Create Bucket

```bash
aws s3api create-bucket \
  --bucket pearii-production-media \
  --region us-east-1
```

### Bucket Policy (public read for media folder)

Create `bucket-policy.json`:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadMedia",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::pearii-production-media/media/*"
    }
  ]
}
```

```bash
aws s3api put-bucket-policy \
  --bucket pearii-production-media \
  --policy file://bucket-policy.json
```

### CORS Configuration

Create `cors.json`:

```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET", "POST", "PUT", "DELETE"],
    "AllowedOrigins": ["https://yourdomain.com"],
    "MaxAgeSeconds": 3600
  }
]
```

```bash
aws s3api put-bucket-cors \
  --bucket pearii-production-media \
  --cors-configuration file://cors.json
```

---

## 🐳 Phase 5 — ECR (Docker Image Registry)

```bash
# Create ECR repository
aws ecr create-repository \
  --repository-name pearii-backend \
  --region us-east-1

# Authenticate Docker to ECR
aws ecr get-login-password --region us-east-1 | \
  docker login --username AWS \
  --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com

# Build image locally
docker build -t pearii-backend .

# Tag and push to ECR
docker tag pearii-backend:latest \
  <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/pearii-backend:latest

docker push \
  <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/pearii-backend:latest
```

---

## 💻 Phase 6 — EC2 Instance Setup

### Launch EC2

```
AWS Console → EC2 → Launch Instance

  AMI:            Ubuntu 24.04 LTS
  Instance type:  t3.medium
  Key pair:       Create new → pearii-key.pem (save safely)
  VPC:            pearii-vpc
  Subnet:         pearii-private-1a
  Security group: pearii-ec2-sg
  Storage:        30 GB gp3
  IAM Role:       Create → attach policies:
                    AmazonEC2ContainerRegistryReadOnly
                    AmazonS3FullAccess
```

> Attaching an IAM role to EC2 lets it pull from ECR and access S3 **without hardcoded credentials**.

### Install Docker on EC2

```bash
# Connect via Session Manager or bastion host
sudo apt update && sudo apt upgrade -y

# Install Docker (official script)
curl -fsSL https://get.docker.com | sudo sh
sudo usermod -aG docker ubuntu
newgrp docker

# Install Docker Compose plugin
sudo apt install -y docker-compose-plugin

# Verify
docker --version
docker compose version
```

### Authenticate EC2 to ECR

```bash
# IAM role on EC2 handles auth — no keys needed
aws ecr get-login-password --region us-east-1 | \
  docker login --username AWS \
  --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
```

---

## 📦 Phase 7 — Docker Files

### `Dockerfile` (Multi-stage Production Build)

```dockerfile
# ── Stage 1: Builder ──────────────────────────────────
FROM python:3.12-slim AS builder

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

WORKDIR /app

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential libpq-dev curl \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .
RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
RUN pip install --upgrade pip && pip install -r requirements.txt

# ── Stage 2: Production ───────────────────────────────
FROM python:3.12-slim

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
ENV PATH="/opt/venv/bin:$PATH"

WORKDIR /app

RUN apt-get update && apt-get install -y --no-install-recommends \
    libpq-dev curl netcat-openbsd \
    && rm -rf /var/lib/apt/lists/*

COPY --from=builder /opt/venv /opt/venv
COPY . .

COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Non-root user for security
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
echo "Database ready."

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

### `docker-compose.prod.yml` (AWS Production — No db/redis containers)

```yaml
version: "3.9"

services:

  # ─── Django App (Gunicorn) ────────────────────────────
  web:
    image: ${ECR_REGISTRY}/pearii-backend:${IMAGE_TAG}
    container_name: pearii_web
    env_file: .env
    volumes:
      - static_volume:/app/staticfiles
    depends_on:
      - nginx
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health/"]
      interval: 30s
      timeout: 10s
      retries: 3
    networks:
      - pearii_network

  # ─── Nginx Reverse Proxy ──────────────────────────────
  nginx:
    image: nginx:1.25-alpine
    container_name: pearii_nginx
    ports:
      - "80:80"
    volumes:
      - ./nginx/conf.d:/etc/nginx/conf.d:ro
      - static_volume:/app/staticfiles:ro
    restart: unless-stopped
    networks:
      - pearii_network

  # ─── Celery Worker ────────────────────────────────────
  celery_worker:
    image: ${ECR_REGISTRY}/pearii-backend:${IMAGE_TAG}
    container_name: pearii_celery_worker
    command: celery -A core worker --loglevel=info --concurrency=4
    env_file: .env
    restart: unless-stopped
    networks:
      - pearii_network

  # ─── Celery Beat Scheduler ────────────────────────────
  celery_beat:
    image: ${ECR_REGISTRY}/pearii-backend:${IMAGE_TAG}
    container_name: pearii_celery_beat
    command: >
      celery -A core beat --loglevel=info
      --scheduler django_celery_beat.schedulers:DatabaseScheduler
    env_file: .env
    restart: unless-stopped
    networks:
      - pearii_network

volumes:
  static_volume:

networks:
  pearii_network:
    driver: bridge
```

### `docker-compose.yml` (Local Development — Includes db & redis)

```yaml
version: "3.9"

services:

  web:
    build: .
    container_name: pearii_web_dev
    env_file: .env
    volumes:
      - .:/app
      - static_volume:/app/staticfiles
      - media_volume:/app/media
    ports:
      - "8000:8000"
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_healthy
    restart: unless-stopped
    networks:
      - pearii_network

  db:
    image: postgres:16-alpine
    container_name: pearii_db_dev
    environment:
      POSTGRES_DB: ${DB_NAME}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER}"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks:
      - pearii_network

  redis:
    image: redis:7-alpine
    container_name: pearii_redis_dev
    volumes:
      - redis_data:/data
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 3s
      retries: 3
    networks:
      - pearii_network

  celery_worker:
    build: .
    container_name: pearii_celery_dev
    command: celery -A core worker --loglevel=info
    env_file: .env
    depends_on:
      - redis
      - db
    networks:
      - pearii_network

  nginx:
    image: nginx:1.25-alpine
    container_name: pearii_nginx_dev
    ports:
      - "80:80"
    volumes:
      - ./nginx/conf.d:/etc/nginx/conf.d:ro
      - static_volume:/app/staticfiles:ro
      - media_volume:/app/media:ro
    depends_on:
      - web
    networks:
      - pearii_network

volumes:
  postgres_data:
  redis_data:
  static_volume:
  media_volume:

networks:
  pearii_network:
    driver: bridge
```

---

## 🌐 Phase 8 — Nginx Configuration

### `nginx/conf.d/default.conf`

```nginx
upstream django_app {
    server web:8000;
}

server {
    listen 80;
    server_name _;

    client_max_body_size 50M;

    # ALB health check endpoint
    location /health/ {
        proxy_pass http://django_app;
        access_log off;
    }

    # Static files (fallback — main static goes to S3)
    location /static/ {
        alias /app/staticfiles/;
        expires 30d;
        access_log off;
        add_header Cache-Control "public, max-age=2592000";
    }

    # All other requests → Django
    location / {
        proxy_pass          http://django_app;
        proxy_set_header    Host              $host;
        proxy_set_header    X-Real-IP         $remote_addr;
        proxy_set_header    X-Forwarded-For   $proxy_add_x_forwarded_for;
        proxy_set_header    X-Forwarded-Proto $http_x_forwarded_proto;
        proxy_read_timeout  120s;
        proxy_connect_timeout 10s;
        proxy_send_timeout  120s;
    }
}
```

> The ALB handles HTTPS (port 443) and SSL termination. It forwards plain HTTP to Nginx on port 80. The `X-Forwarded-Proto` header tells Django the original request was HTTPS.

---

## 🔐 Phase 9 — Environment Variables

Create `.env` on EC2 at `/opt/pearii/.env`:

```env
# ── Django Core ───────────────────────────────────────
SECRET_KEY=your-very-long-random-secret-key-here
DEBUG=False
DJANGO_SETTINGS_MODULE=core.settings.production
ALLOWED_HOSTS=yourdomain.com,www.yourdomain.com

# ── RDS PostgreSQL ────────────────────────────────────
DB_ENGINE=django.db.backends.postgresql
DB_NAME=pearii_db
DB_USER=pearii_user
DB_PASSWORD=your-rds-master-password
DB_HOST=pearii-db.xxxxxxxxx.us-east-1.rds.amazonaws.com
DB_PORT=5432

# ── ElastiCache Redis ─────────────────────────────────
REDIS_URL=redis://pearii-redis.xxxxxx.cache.amazonaws.com:6379/0
CELERY_BROKER_URL=redis://pearii-redis.xxxxxx.cache.amazonaws.com:6379/0
CELERY_RESULT_BACKEND=redis://pearii-redis.xxxxxx.cache.amazonaws.com:6379/0

# ── AWS S3 ────────────────────────────────────────────
USE_S3=True
AWS_STORAGE_BUCKET_NAME=pearii-production-media
AWS_S3_REGION_NAME=us-east-1
# No AWS keys needed — EC2 IAM Role handles S3 authentication

# ── ECR Image ─────────────────────────────────────────
ECR_REGISTRY=<AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
IMAGE_TAG=latest

# ── Firebase ──────────────────────────────────────────
FIREBASE_CREDENTIALS_PATH=/app/peariiapp-firebase-adminsdk.json

# ── AI / OpenAI ───────────────────────────────────────
OPENAI_API_KEY=sk-your-openai-key
ANTHROPIC_API_KEY=sk-ant-your-anthropic-key

# ── Email ─────────────────────────────────────────────
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USE_TLS=True
EMAIL_HOST_USER=your-email@gmail.com
EMAIL_HOST_PASSWORD=your-app-password
```

> Generate a strong secret key:
> ```bash
> python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
> ```

---

## ⚙️ Phase 10 — Django Production Settings

```python
# core/settings/production.py
import environ

env = environ.Env()

DEBUG = False
SECRET_KEY = env("SECRET_KEY")
ALLOWED_HOSTS = env.list("ALLOWED_HOSTS")

# Tell Django it's behind the ALB proxy
USE_X_FORWARDED_HOST = True
SECURE_PROXY_SSL_HEADER = ("HTTP_X_FORWARDED_PROTO", "https")

# ── Security Headers ──────────────────────────────────
SECURE_SSL_REDIRECT             = True
SECURE_HSTS_SECONDS             = 63072000
SECURE_HSTS_INCLUDE_SUBDOMAINS  = True
SECURE_HSTS_PRELOAD             = True
SESSION_COOKIE_SECURE           = True
CSRF_COOKIE_SECURE              = True
X_FRAME_OPTIONS                 = "DENY"
SECURE_CONTENT_TYPE_NOSNIFF     = True

# ── RDS PostgreSQL ────────────────────────────────────
DATABASES = {
    "default": {
        "ENGINE":       "django.db.backends.postgresql",
        "NAME":         env("DB_NAME"),
        "USER":         env("DB_USER"),
        "PASSWORD":     env("DB_PASSWORD"),
        "HOST":         env("DB_HOST"),
        "PORT":         env("DB_PORT", default="5432"),
        "CONN_MAX_AGE": 60,
        "OPTIONS":      {"sslmode": "require"},
    }
}

# ── ElastiCache Redis Cache ───────────────────────────
CACHES = {
    "default": {
        "BACKEND":  "django_redis.cache.RedisCache",
        "LOCATION": env("REDIS_URL"),
        "OPTIONS":  {"CLIENT_CLASS": "django_redis.client.DefaultClient"},
    }
}
SESSION_ENGINE     = "django.contrib.sessions.backends.cache"
SESSION_CACHE_ALIAS = "default"

# ── Celery ────────────────────────────────────────────
CELERY_BROKER_URL     = env("CELERY_BROKER_URL")
CELERY_RESULT_BACKEND = env("CELERY_RESULT_BACKEND")
CELERY_ACCEPT_CONTENT = ["json"]
CELERY_TASK_SERIALIZER = "json"

# ── AWS S3 Storage ────────────────────────────────────
if env.bool("USE_S3", default=False):
    DEFAULT_FILE_STORAGE = "storages.backends.s3boto3.S3Boto3Storage"
    STATICFILES_STORAGE  = "storages.backends.s3boto3.S3StaticStorage"

    AWS_STORAGE_BUCKET_NAME  = env("AWS_STORAGE_BUCKET_NAME")
    AWS_S3_REGION_NAME       = env("AWS_S3_REGION_NAME", default="us-east-1")
    AWS_S3_CUSTOM_DOMAIN     = f"{AWS_STORAGE_BUCKET_NAME}.s3.amazonaws.com"
    AWS_DEFAULT_ACL          = "public-read"
    AWS_S3_OBJECT_PARAMETERS = {"CacheControl": "max-age=86400"}
    # No AWS_ACCESS_KEY_ID — EC2 IAM role handles auth

    STATIC_URL = f"https://{AWS_S3_CUSTOM_DOMAIN}/static/"
    MEDIA_URL  = f"https://{AWS_S3_CUSTOM_DOMAIN}/media/"

# ── Logging ───────────────────────────────────────────
LOGGING = {
    "version": 1,
    "disable_existing_loggers": False,
    "handlers": {
        "console": {"class": "logging.StreamHandler"},
    },
    "root": {
        "handlers": ["console"],
        "level": "INFO",
    },
}
```

### Health Check Endpoint

Add to `api/urls.py` or `core/urls.py`:

```python
from django.http import JsonResponse

def health_check(request):
    return JsonResponse({"status": "ok", "service": "pearii-backend"})

urlpatterns += [
    path("health/", health_check),
]
```

---

## 🚀 Phase 11 — First Deployment on EC2

```bash
# Clone project to EC2
git clone https://github.com/shagorrobidas/pearii-backend.git /opt/pearii
cd /opt/pearii

# Create .env file with production values
nano .env

# Pull Docker image from ECR
aws ecr get-login-password --region us-east-1 | \
  docker login --username AWS \
  --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com

docker compose -f docker-compose.prod.yml pull

# Start all containers
docker compose -f docker-compose.prod.yml up -d

# Verify containers are running
docker compose -f docker-compose.prod.yml ps

# Run migrations
docker compose -f docker-compose.prod.yml exec web \
  python manage.py migrate --noinput

# Push static files to S3
docker compose -f docker-compose.prod.yml exec web \
  python manage.py collectstatic --noinput

# Create admin superuser
docker compose -f docker-compose.prod.yml exec web \
  python manage.py createsuperuser

# Check logs
docker compose -f docker-compose.prod.yml logs -f web
```

---

## 🔁 Phase 12 — Auto-start on EC2 Reboot

```bash
sudo nano /etc/systemd/system/pearii.service
```

```ini
[Unit]
Description=Pearii Docker Compose Stack
Requires=docker.service
After=docker.service network-online.target

[Service]
Type=oneshot
RemainAfterExit=yes
WorkingDirectory=/opt/pearii
ExecStart=docker compose -f docker-compose.prod.yml up -d
ExecStop=docker compose -f docker-compose.prod.yml down
TimeoutStartSec=300

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl enable pearii
sudo systemctl start pearii
```

---

## ⚖️ Phase 13 — Application Load Balancer Setup

```
AWS Console → EC2 → Load Balancers → Create Application Load Balancer

  Name:     pearii-alb
  Scheme:   Internet-facing
  VPC:      pearii-vpc
  Subnets:  pearii-public-1a, pearii-public-1b (both public)
  SG:       pearii-alb-sg

Listeners:
  HTTP  :80  → Redirect to HTTPS (301)
  HTTPS :443 → Forward to target group

Target Group:
  Name:         pearii-tg
  Type:         Instances
  Protocol:     HTTP
  Port:         80    ← hits Nginx container
  Health check: GET /health/
  Threshold:    2 healthy, 3 unhealthy
  Register:     your EC2 instance

SSL Certificate:
  Select your ACM certificate (see Phase 14)
```

---

## 🔒 Phase 14 — SSL Certificate (ACM) + Route 53

### Request SSL Certificate

```
AWS Console → Certificate Manager → Request certificate
  Type:    Public
  Domains: yourdomain.com
           *.yourdomain.com
  Method:  DNS validation
  → ACM gives you CNAME records to add in Route 53
  → Once validated (few minutes), attach to ALB HTTPS listener
```

### Route 53 DNS

```
Route 53 → Hosted zones → Create hosted zone
  Domain: yourdomain.com

Create A record:
  Name:          @ (root)
  Type:          A
  Alias:         Yes
  Target:        pearii-alb-xxxxx.us-east-1.elb.amazonaws.com

Create A record:
  Name:          www
  Type:          A
  Alias:         Yes
  Target:        same ALB DNS name
```

---

## 🔄 Phase 15 — CI/CD with GitHub Actions + ECR

### `.github/workflows/deploy.yml`

```yaml
name: Build → ECR → Deploy to EC2

on:
  push:
    branches: [main]

env:
  AWS_REGION: us-east-1
  ECR_REPO:   pearii-backend

jobs:

  # ── Run Tests ─────────────────────────────────────────
  test:
    name: Django Tests
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with: { python-version: "3.12" }
      - run: pip install -r requirements.txt
      - run: python manage.py test --verbosity=2
        env:
          DJANGO_SETTINGS_MODULE: core.settings.development
          SECRET_KEY: ci-test-only-key
          DB_HOST: localhost

  # ── Build & Push Docker Image to ECR ─────────────────
  build:
    name: Build & Push to ECR
    needs: test
    runs-on: ubuntu-latest
    outputs:
      image: ${{ steps.push.outputs.image }}

    steps:
      - uses: actions/checkout@v4

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id:     ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region:            ${{ env.AWS_REGION }}

      - name: Login to ECR
        id: ecr-login
        uses: aws-actions/amazon-ecr-login@v2

      - name: Build, tag, push image
        id: push
        env:
          REGISTRY:  ${{ steps.ecr-login.outputs.registry }}
          IMAGE_TAG: ${{ github.sha }}
        run: |
          docker build -t $REGISTRY/$ECR_REPO:$IMAGE_TAG .
          docker tag $REGISTRY/$ECR_REPO:$IMAGE_TAG $REGISTRY/$ECR_REPO:latest
          docker push $REGISTRY/$ECR_REPO:$IMAGE_TAG
          docker push $REGISTRY/$ECR_REPO:latest
          echo "image=$REGISTRY/$ECR_REPO:$IMAGE_TAG" >> $GITHUB_OUTPUT

  # ── Deploy to EC2 via SSH ─────────────────────────────
  deploy:
    name: Deploy to EC2
    needs: build
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Deploy via SSH
        uses: appleboy/ssh-action@v1.0.0
        with:
          host:     ${{ secrets.EC2_HOST }}
          username: ubuntu
          key:      ${{ secrets.EC2_SSH_KEY }}
          script: |
            cd /opt/pearii
            git pull origin main

            # Re-authenticate to ECR
            aws ecr get-login-password --region us-east-1 | \
              docker login --username AWS \
              --password-stdin ${{ secrets.ECR_REGISTRY }}

            # Set image tag
            export ECR_REGISTRY=${{ secrets.ECR_REGISTRY }}
            export IMAGE_TAG=${{ github.sha }}

            # Pull new image
            docker compose -f docker-compose.prod.yml pull

            # Rolling restart of app containers only
            docker compose -f docker-compose.prod.yml up -d \
              --no-deps web celery_worker celery_beat

            # Post-deploy
            docker compose -f docker-compose.prod.yml exec -T web \
              python manage.py migrate --noinput

            docker compose -f docker-compose.prod.yml exec -T web \
              python manage.py collectstatic --noinput

            # Cleanup old images
            docker image prune -f

            echo "✅ Deployed: ${{ github.sha }}"
```

### Required GitHub Secrets

| Secret | Value |
|---|---|
| `AWS_ACCESS_KEY_ID` | IAM user key (for CI/CD only) |
| `AWS_SECRET_ACCESS_KEY` | IAM user secret |
| `ECR_REGISTRY` | `<account-id>.dkr.ecr.us-east-1.amazonaws.com` |
| `EC2_HOST` | EC2 private IP or Bastion host IP |
| `EC2_SSH_KEY` | Private SSH key content |

---

## 📦 Phase 16 — Required Python Packages

Add these to `requirements.txt`:

```txt
django>=5.0
djangorestframework>=3.15
gunicorn>=21.0
psycopg2-binary>=2.9
redis>=5.0
django-redis>=5.4
celery>=5.3
django-celery-beat>=2.6
django-storages>=1.14
boto3>=1.34
django-environ>=0.11
django-prometheus>=2.3
```

---

## 🗃 Common Docker Commands on EC2

```bash
# ─── View all containers ─────────────────────────────
docker compose -f docker-compose.prod.yml ps

# ─── Live logs ───────────────────────────────────────
docker compose -f docker-compose.prod.yml logs -f web
docker compose -f docker-compose.prod.yml logs -f celery_worker

# ─── Run Django management commands ──────────────────
docker compose -f docker-compose.prod.yml exec web python manage.py migrate
docker compose -f docker-compose.prod.yml exec web python manage.py shell
docker compose -f docker-compose.prod.yml exec web python manage.py createsuperuser
docker compose -f docker-compose.prod.yml exec web python manage.py check --deploy

# ─── Restart a single container ──────────────────────
docker compose -f docker-compose.prod.yml restart web
docker compose -f docker-compose.prod.yml restart celery_worker

# ─── Full redeploy ────────────────────────────────────
docker compose -f docker-compose.prod.yml pull
docker compose -f docker-compose.prod.yml up -d

# ─── Check container resource usage ──────────────────
docker stats

# ─── Cleanup unused images ───────────────────────────
docker image prune -f
docker system prune -f

# ─── Stop everything ─────────────────────────────────
docker compose -f docker-compose.prod.yml down
```

---

## 🔐 Production Security Checklist

### Django

```python
# Verify all these are set in production.py
DEBUG = False                                   # ✅
SECRET_KEY = env("SECRET_KEY")                  # ✅ from env
SECURE_SSL_REDIRECT = True                      # ✅
SECURE_HSTS_SECONDS = 63072000                  # ✅
SESSION_COOKIE_SECURE = True                    # ✅
CSRF_COOKIE_SECURE = True                       # ✅
X_FRAME_OPTIONS = "DENY"                        # ✅
ALLOWED_HOSTS = env.list("ALLOWED_HOSTS")       # ✅
```

### Docker

```bash
# ✅ Containers run as non-root (django user in Dockerfile)
# ✅ No DB/Redis ports exposed to public
# ✅ ECR images scanned for vulnerabilities
# ✅ .env file never committed to git
# ✅ EC2 uses IAM Role instead of hardcoded keys

# Scan Docker image for vulnerabilities
docker scout cves pearii-backend:latest
```

### AWS Security

```bash
# ✅ RDS in private subnet — no public access
# ✅ ElastiCache in private subnet
# ✅ Security groups locked to minimum required ports
# ✅ ALB handles SSL — EC2 never exposed on 443
# ✅ S3 bucket blocks public PUT (only GET for media/)
# ✅ CloudTrail enabled for audit logging
# ✅ VPC Flow Logs enabled

# Enable S3 versioning for media protection
aws s3api put-bucket-versioning \
  --bucket pearii-production-media \
  --versioning-configuration Status=Enabled
```

---

## 💡 Best Practices

### Performance

- Use `CONN_MAX_AGE=60` in Django database settings to reuse DB connections
- Enable Redis caching for expensive API responses
- Serve static files from S3 + CloudFront (not Django)
- Scale Gunicorn workers: `workers = (2 × CPU cores) + 1`
- Use `gthread` worker class for I/O-bound Django apps

### Cost Optimization

- Use `t3.medium` for EC2 and scale up only when needed
- Enable RDS storage autoscaling (pay for what you use)
- Set S3 lifecycle rules to move old media to Glacier
- Use Reserved Instances for RDS/EC2 to save 30–60%

### Reliability

- Enable Multi-AZ on RDS for automatic failover
- Set `restart: unless-stopped` on all Docker containers
- Use ALB health checks to auto-remove unhealthy EC2 instances
- Set up CloudWatch alarms for CPU, memory, and error rates

### Database Backups

```bash
# Manual RDS backup via pg_dump through EC2
docker compose -f docker-compose.prod.yml exec web \
  python -c "
import subprocess
subprocess.run([
  'pg_dump',
  '-h', 'pearii-db.xxx.us-east-1.rds.amazonaws.com',
  '-U', 'pearii_user',
  '-d', 'pearii_db',
  '-f', '/app/backup.sql'
])
"

# Or use AWS automated backups (already configured in RDS — 7 day retention)
```

---

## 🧪 Verify Everything Works

```bash
# 1. Check all containers are up
docker compose -f docker-compose.prod.yml ps
# Expected: web, nginx, celery_worker, celery_beat → all Up

# 2. Test health endpoint
curl http://localhost/health/
# Expected: {"status": "ok", "service": "pearii-backend"}

# 3. Test Django can reach RDS
docker compose -f docker-compose.prod.yml exec web \
  python manage.py dbshell
# Expected: drops into PostgreSQL shell

# 4. Test Redis connection
docker compose -f docker-compose.prod.yml exec web \
  python -c "import django; django.setup(); from django.core.cache import cache; cache.set('test', 'ok'); print(cache.get('test'))"
# Expected: ok

# 5. Test S3 connection
docker compose -f docker-compose.prod.yml exec web \
  python manage.py collectstatic --dry-run
# Expected: lists files to be uploaded to S3

# 6. Test from public internet
curl -I https://yourdomain.com/health/
# Expected: HTTP/2 200
```

---

## 📄 License

This project is licensed under the **MIT License** — see [LICENSE](LICENSE) for details.

---

<div align="center">

**Built with ❤️ by [Shagor Robidas](https://github.com/shagorrobidas)**

⭐ Star this repo if it helped you! ⭐

</div>
