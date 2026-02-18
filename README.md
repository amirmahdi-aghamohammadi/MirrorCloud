
# Registry

**Registry** is a **Private Docker Registry** with an **S3 backend** and **Pull-Through Cache**, designed to securely store Docker images in cloud object storage and automatically fetch missing images from Docker Hub.

![Docker](https://img.shields.io/badge/docker-ready-blue)
![S3](https://img.shields.io/badge/storage-S3-orange)
![License](https://img.shields.io/badge/license-MIT-green)

## What You Can Do

* Push Docker images to your private registry
* Automatically proxy and cache images from Docker Hub
* Store all layers and blobs in S3-compatible Object Storage
* Run easily using Docker Compose
* Support multi-architecture Docker images

## Setup

### Clone the repository
```bash
git clone https://github.com/amirmahdi-aghamohammadi/MirrorCloud.git
```

### Configure environment variablesپ

Create a `.env` file:

```dotenv
AWS_ACCESS_KEY=your_access_key
AWS_SECRET_KEY=your_secret_key
S3_BUCKET=your_bucket_name
S3_REGION=ir-thr-at1
S3_ENDPOINT=https://S3_endpoint
REGISTRY_HTTP_SECRET=mysupersecretstring
REGISTRY_PORT=5000
```

###  Generate `config.yml`

```bash
export $(cat .env | xargs)
envsubst < config.yml > config.generated.yml
mv config.generated.yml config.yml
```

### Launch with Docker Compose

```bash
docker compose up -d
```

Verify:

```bash
docker ps
```

## Testing

### Pull an image through the registry

```bash
docker pull IP:5000/library/nginx:latest
```

* First pull → fetched from Docker Hub and stored in S3
* Next pulls → served directly from S3

Check catalog:

```bash
curl http://IP:5000/v2/_catalog
```

Expected:

```json
{"repositories":["library/nginx"]}
```

### Push an image

```bash
docker tag nginx:latest IP:5000/nginx:test
docker push IP:5000/nginx:test
```

## Features

* **Pull-Through Cache** – Automatically fetch and cache missing images
* **S3 Storage Backend** – Durable and scalable object storage
* **Multi-Arch Support** – Works with Docker multi-platform images
* **Cloud Native Ready** – Stateless design with object storage backend
* **Lightweight & Simple Deployment**
