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
