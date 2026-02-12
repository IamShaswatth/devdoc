# Docker Documentation

## Table of Contents
- [Introduction](#introduction)
- [Docker Installation](#docker-installation)
- [Basic Docker Commands](#basic-docker-commands)
- [Docker Image Commands](#docker-image-commands)
- [Docker Container Commands](#docker-container-commands)
- [Docker Network Commands](#docker-network-commands)
- [Docker Volume Commands](#docker-volume-commands)
- [Dockerfile Commands](#dockerfile-commands)
- [Docker Compose Commands](#docker-compose-commands)
- [Docker System Commands](#docker-system-commands)

## Introduction

Docker is a platform for developing, shipping, and running applications in containers. Containers allow you to package an application with all its dependencies into a standardized unit for software development.

## Docker Installation

### Install Docker on Linux
```bash
# Update package index
sudo apt-get update

# Install Docker
sudo apt-get install docker.io

# Start Docker service
sudo systemctl start docker

# Enable Docker to start on boot
sudo systemctl enable docker

# Verify installation
docker --version
```

### Add user to Docker group (to run without sudo)
```bash
sudo usermod -aG docker $USER
```

## Basic Docker Commands

### Check Docker Version
```bash
docker --version
docker version
```

### Display Docker System Information
```bash
docker info
```

### Get Help
```bash
docker --help
docker <command> --help
```

## Docker Image Commands

### List Images
```bash
# List all images
docker images

# List all images (including intermediate)
docker images -a
```

### Pull an Image from Docker Hub
```bash
# Pull latest version
docker pull <image-name>

# Pull specific version
docker pull <image-name>:<tag>

# Examples
docker pull ubuntu
docker pull nginx:latest
docker pull node:18
```

### Build an Image from Dockerfile
```bash
# Build image from current directory
docker build -t <image-name>:<tag> .

# Build with specific Dockerfile
docker build -f Dockerfile -t <image-name> .

# Example
docker build -t myapp:1.0 .
```

### Remove Images
```bash
# Remove single image
docker rmi <image-id>

# Remove multiple images
docker rmi <image-id1> <image-id2>

# Remove all unused images
docker image prune

# Remove all images
docker rmi $(docker images -q)
```

### Tag an Image
```bash
docker tag <source-image>:<tag> <target-image>:<tag>
```

### Push Image to Docker Hub
```bash
# Login to Docker Hub
docker login

# Push image
docker push <username>/<image-name>:<tag>
```

### Search for Images
```bash
docker search <image-name>
```

### Inspect an Image
```bash
docker image inspect <image-name>
```

### View Image History
```bash
docker history <image-name>
```

## Docker Container Commands

### Run a Container
```bash
# Run container in foreground
docker run <image-name>

# Run container in background (detached mode)
docker run -d <image-name>

# Run container with custom name
docker run --name <container-name> <image-name>

# Run with port mapping
docker run -p <host-port>:<container-port> <image-name>

# Run with environment variables
docker run -e KEY=VALUE <image-name>

# Run with volume mount
docker run -v <host-path>:<container-path> <image-name>

# Run interactive container with terminal
docker run -it <image-name> /bin/bash

# Example: Run nginx
docker run -d -p 8080:80 --name my-nginx nginx
```

### List Containers
```bash
# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# List latest created container
docker ps -l
```

### Stop Container
```bash
# Stop single container
docker stop <container-id>

# Stop multiple containers
docker stop <container-id1> <container-id2>

# Stop all running containers
docker stop $(docker ps -q)
```

### Start Container
```bash
docker start <container-id>
```

### Restart Container
```bash
docker restart <container-id>
```

### Remove Container
```bash
# Remove stopped container
docker rm <container-id>

# Force remove running container
docker rm -f <container-id>

# Remove all stopped containers
docker container prune

# Remove all containers
docker rm $(docker ps -aq)
```

### Execute Command in Running Container
```bash
# Execute command
docker exec <container-id> <command>

# Interactive shell
docker exec -it <container-id> /bin/bash
docker exec -it <container-id> sh
```

### View Container Logs
```bash
# View logs
docker logs <container-id>

# Follow logs (real-time)
docker logs -f <container-id>

# View last N lines
docker logs --tail 100 <container-id>
```

### Inspect Container
```bash
docker inspect <container-id>
```

### View Container Resource Usage
```bash
# Real-time stats
docker stats

# Stats for specific container
docker stats <container-id>
```

### Copy Files Between Container and Host
```bash
# Copy from container to host
docker cp <container-id>:<container-path> <host-path>

# Copy from host to container
docker cp <host-path> <container-id>:<container-path>
```

### Pause/Unpause Container
```bash
docker pause <container-id>
docker unpause <container-id>
```

### Rename Container
```bash
docker rename <old-name> <new-name>
```

### View Container Processes
```bash
docker top <container-id>
```

## Docker Network Commands

### List Networks
```bash
docker network ls
```

### Create Network
```bash
docker network create <network-name>

# Create with specific driver
docker network create --driver bridge <network-name>
```

### Inspect Network
```bash
docker network inspect <network-name>
```

### Connect Container to Network
```bash
docker network connect <network-name> <container-id>
```

### Disconnect Container from Network
```bash
docker network disconnect <network-name> <container-id>
```

### Remove Network
```bash
docker network rm <network-name>

# Remove all unused networks
docker network prune
```

## Docker Volume Commands

### List Volumes
```bash
docker volume ls
```

### Create Volume
```bash
docker volume create <volume-name>
```

### Inspect Volume
```bash
docker volume inspect <volume-name>
```

### Remove Volume
```bash
docker volume rm <volume-name>

# Remove all unused volumes
docker volume prune
```

### Run Container with Volume
```bash
# Named volume
docker run -v <volume-name>:<container-path> <image-name>

# Bind mount
docker run -v <host-path>:<container-path> <image-name>

# Read-only volume
docker run -v <volume-name>:<container-path>:ro <image-name>
```

## Dockerfile Commands

### Common Dockerfile Instructions

```dockerfile
# Base image
FROM ubuntu:20.04

# Set maintainer
LABEL maintainer="your-email@example.com"

# Set working directory
WORKDIR /app

# Copy files
COPY . /app
COPY package.json .

# Add files (with extraction support)
ADD archive.tar.gz /app

# Run commands during build
RUN apt-get update && apt-get install -y nodejs

# Set environment variables
ENV NODE_ENV=production
ENV PORT=3000

# Expose ports
EXPOSE 3000

# Set user
USER node

# Define volume mount points
VOLUME ["/data"]

# Entry point (always executed)
ENTRYPOINT ["node"]

# Default command (can be overridden)
CMD ["app.js"]

# Health check
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost/ || exit 1

# Arguments during build
ARG VERSION=latest
```

## Docker Compose Commands

### Start Services
```bash
# Start in foreground
docker-compose up

# Start in background
docker-compose up -d

# Build and start
docker-compose up --build
```

### Stop Services
```bash
docker-compose stop
```

### Down Services (stop and remove)
```bash
docker-compose down

# Remove volumes too
docker-compose down -v
```

### View Running Services
```bash
docker-compose ps
```

### View Logs
```bash
# All services
docker-compose logs

# Specific service
docker-compose logs <service-name>

# Follow logs
docker-compose logs -f
```

### Execute Command in Service
```bash
docker-compose exec <service-name> <command>
```

### Build Services
```bash
docker-compose build
```

### Pull Images
```bash
docker-compose pull
```

### Restart Services
```bash
docker-compose restart
```

### Scale Services
```bash
docker-compose up -d --scale <service-name>=3
```

## Docker System Commands

### View Disk Usage
```bash
docker system df
```

### Clean Up Everything
```bash
# Remove all unused containers, networks, images
docker system prune

# Remove all (including volumes)
docker system prune -a --volumes
```

### Remove All Stopped Containers
```bash
docker container prune
```

### Remove All Unused Images
```bash
docker image prune

# Remove all images
docker image prune -a
```

### Remove All Unused Networks
```bash
docker network prune
```

### Remove All Unused Volumes
```bash
docker volume prune
```

### Login to Docker Registry
```bash
docker login

# Login to specific registry
docker login <registry-url>
```

### Logout from Docker Registry
```bash
docker logout
```

## Useful Docker Command Combinations

### Stop and Remove All Containers
```bash
docker stop $(docker ps -aq) && docker rm $(docker ps -aq)
```

### Remove All Images
```bash
docker rmi $(docker images -q)
```

### Remove Dangling Images
```bash
docker rmi $(docker images -f "dangling=true" -q)
```

### Enter Running Container
```bash
docker exec -it $(docker ps -q -f name=<container-name>) /bin/bash
```

### View Container IP Address
```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container-id>
```

### Monitor Container Logs in Real-time
```bash
docker logs -f --tail 100 <container-id>
```

## Best Practices

1. **Keep Images Small**: Use minimal base images like Alpine Linux
2. **Use .dockerignore**: Exclude unnecessary files from build context
3. **Layer Caching**: Order Dockerfile commands from least to most frequently changing
4. **One Process per Container**: Follow microservices architecture
5. **Use Official Images**: Start with official base images from Docker Hub
6. **Security**: Don't run containers as root, scan images for vulnerabilities
7. **Health Checks**: Implement health checks in Dockerfile
8. **Environment Variables**: Use env variables for configuration
9. **Volumes for Data**: Use volumes for persistent data
10. **Multi-stage Builds**: Reduce final image size using multi-stage builds

## Troubleshooting

### Container Won't Start
```bash
# Check logs
docker logs <container-id>

# Inspect container
docker inspect <container-id>
```

### Out of Disk Space
```bash
# Clean up unused resources
docker system prune -a
```

### Permission Denied
```bash
# Add user to docker group
sudo usermod -aG docker $USER
```

### Port Already in Use
```bash
# Find process using port
sudo lsof -i :8080

# Use different port mapping
docker run -p 8081:80 nginx
```

## Additional Resources

- [Docker Official Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)

---

*Last Updated: February 12, 2026*
