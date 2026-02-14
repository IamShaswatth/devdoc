# Docker Learning Documentation

A comprehensive guide to Docker for containerization and application deployment.

## Table of Contents
- [Introduction to Docker](#introduction-to-docker)
- [Docker Installation](#docker-installation)
- [Docker Image Commands](#docker-image-commands)
- [Hands-On: Running Nginx Container](#hands-on-running-nginx-container)

---

<details>
<summary><b>Introduction to Docker</b></summary>

### Introduction

Docker is a platform for developing, shipping, and running applications in containers. Containers allow you to package an application with all its dependencies into a standardized unit for software development.

</details>

<details>
<summary><b>Docker Installation</b></summary>

### Docker Installation

#### Install Docker on Linux
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

#### Add user to Docker group (to run without sudo)
```bash
sudo usermod -aG docker $USER
```

</details>

<details>
<summary><b>Docker Image Commands</b></summary>

### Docker Image Commands

#### List Images
```bash
# List all images
docker images

# List all images (including intermediate)
docker images -a
```

#### Pull an Image from Docker Hub
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

#### Build an Image from Dockerfile
```bash
# Build image from current directory
docker build -t <image-name>:<tag> .

# Build with specific Dockerfile
docker build -f Dockerfile -t <image-name> .

# Example
docker build -t myapp:1.0 .
```

#### Remove Images
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

#### Tag an Image
```bash
docker tag <source-image>:<tag> <target-image>:<tag>
```

#### Push Image to Docker Hub
```bash
# Login to Docker Hub
docker login

# Push image
docker push <username>/<image-name>:<tag>
```

#### Other Image Commands
```bash
# Search for images
docker search <image-name>

# Inspect an image
docker image inspect <image-name>

# View image history
docker history <image-name>
```

</details>

<details>
<summary><b>Hands-On: Running Nginx Container</b></summary>

### Hands-On: Running Nginx Container

This practical guide walks you through creating and running an Nginx web server container, and modifying its content using Docker commands.

#### Step 1: Pull and Create Your First Container

First, let's pull the Nginx image and create your first container:

```bash
# Pull nginx image from Docker Hub
docker pull nginx

# Run nginx container with port mapping
docker run -d -p 8080:80 --name my-nginx nginx

# Verify the container is running
docker ps
```

![First Container Running](docker%20images/first%20container.png)

**What's happening here:**
- `-d`: Runs the container in detached mode (background)
- `-p 8080:80`: Maps port 8080 on your host to port 80 in the container
- `--name my-nginx`: Gives the container a friendly name
- The container is now serving the default Nginx welcome page at `http://localhost:8080`

#### Step 2: Using Docker Exec to Modify Content

Now let's use `docker exec` to access the running container and modify the Nginx HTML content:

```bash
# Access the container with an interactive bash shell
docker exec -it my-nginx bash

# Inside the container, navigate to the HTML directory
cd /usr/share/nginx/html

# View the current content
cat index.html

# Modify the HTML file (you might need to install an editor first)
echo "<h1>Hello from Docker!</h1>" > index.html

# Exit the container
exit
```

![Using Docker Exec Command](docker%20images/docker%20exec.png)

**Alternative: Modify without entering the container:**
```bash
# Execute a single command to update the HTML
docker exec my-nginx bash -c "echo '<h1>Modified with Docker Exec!</h1>' > /usr/share/nginx/html/index.html"
```

#### Step 3: Verify the Changes

Visit `http://localhost:8080` in your browser to see your changes, or use curl:

```bash
curl http://localhost:8080
```

#### Step 4: Working with Container Files

```bash
# Copy files from host to container
docker cp ./myfile.html my-nginx:/usr/share/nginx/html/

# Copy files from container to host
docker cp my-nginx:/usr/share/nginx/html/index.html ./backup.html

# View container logs
docker logs my-nginx

# View real-time logs
docker logs -f my-nginx
```

#### Step 5: Container Management

```bash
# Stop the container
docker stop my-nginx

# Start it again
docker start my-nginx

# Restart the container
docker restart my-nginx

# Remove the container (must be stopped first)
docker stop my-nginx
docker rm my-nginx
```

#### Key Takeaways

- **docker exec**: Allows you to run commands inside a running container
- **Port Mapping**: `-p host_port:container_port` makes services accessible from your host
- **Detached Mode**: `-d` runs containers in the background
- **Interactive Mode**: `-it` provides an interactive terminal session

</details>

---

## Conclusion

This documentation covers the essential concepts for practical Docker containerization with hands-on examples. Docker provides a powerful platform for developing, shipping, and running applications in isolated, portable containers.

---

*Last Updated: February 12, 2026*
