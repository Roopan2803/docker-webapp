# Docker Web Application

## Overview
Dockerized a simple web application using Nginx and deployed it using Docker. Built a custom Docker image and ran it as a container with port mapping.

## Tools Used
- Docker
- Nginx
- HTML/CSS

## Project Structure
- Dockerfile - Docker image configuration
- docker-compose.yml - Multi-container setup
- app/index.html - Web application

## What This Project Does
- Builds a custom Docker image using Nginx as base
- Copies custom HTML page into the container
- Exposes port 80 and maps to host port 8080
- Runs as a detached container

## Commands Used
docker build -t roopan-webapp .
docker run -d -p 8080:80 --name roopan-webapp roopan-webapp
docker ps
docker stop roopan-webapp
docker rm roopan-webapp

## Docker Compose
docker-compose up -d
docker-compose down

## Result
Web application accessible at http://localhost:8080

## CI/CD Pipeline

This app is now deployed automatically via a Jenkins pipeline running inside a Kubernetes cluster:

**Flow:** `git push` → Jenkins detects the change → Kaniko builds the Docker image (no Docker daemon required, since Jenkins runs as a pod) → image pushed to Docker Hub → `kubectl` deploys the new image to the cluster

See [Jenkinsfile](./Jenkinsfile) for the full pipeline definition.

### Key challenges solved
- **No Docker daemon inside Jenkins pods** — switched from `docker build` to [Kaniko](https://github.com/GoogleContainerTools/kaniko), which builds images without needing Docker itself
- **No `kubectl` in the build environment** — added a dedicated `kubectl` container (`alpine/k8s`) to the pipeline's pod spec, since the Kaniko image only contains build tooling
- **Deploy targeting wrong namespace** — Jenkins pods run in the `jenkins` namespace by default; explicitly specified `-n default` so `kubectl` updates the right Deployment

Related project: [kubernetes-monitoring-stack](https://github.com/Roopan2803/kubernetes-monitoring-stack) — the cluster this app deploys to, complete with Prometheus/Grafana monitoring.
