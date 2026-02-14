# DevOps Learning Documentation

A comprehensive guide to Jenkins and Docker for continuous integration, continuous delivery, and containerization.

## Table of Contents
- [Jenkins - CI/CD Automation](#jenkins---cicd-automation)
- [Docker - Containerization](#docker---containerization)

---

## Jenkins - CI/CD Automation

<details>
<summary><b> Introduction to Jenkins</b></summary>

### Introduction

Jenkins is a popular open-source automation server used for continuous integration and continuous delivery (CI/CD). This documentation provides a step-by-step guide to learning Jenkins and building automated pipelines.

</details>

<details>
<summary><b>Jenkins Installation</b></summary>

### Jenkins Installation

Jenkins can be installed on various platforms including Linux, Windows, and macOS. The installation process involves downloading Jenkins and configuring it to run as a service.

#### Installation Screenshot

![Jenkins Installation](jenkins%20screenshot/jenkins%20intallation.png)

**Key Steps:**
1. Download Jenkins from the official website
2. Install Java (Jenkins requires Java to run)
3. Set up Jenkins and configure initial settings
4. Create admin user and install recommended plugins

</details>

<details>
<summary><b>Creating Your First Build</b></summary>

### Creating Your First Build

Once Jenkins is installed, you can create your first build job to understand how Jenkins automation works.

#### Build Configuration

![My First Build](jenkins%20screenshot/my%20first%20build.png)

**Steps to Create a Build:**
1. Click on "New Item" from the Jenkins dashboard
2. Enter a name for your job
3. Select the job type (Freestyle project, Pipeline, etc.)
4. Configure source code management (Git, SVN, etc.)
5. Add build steps (shell commands, scripts, etc.)
6. Save the configuration

#### Build Output

After running your first build, you can view the console output to see the execution details.

![My First Build Output](jenkins%20screenshot/my%20first%20build%20output.png)

**Understanding Build Output:**
- Console output shows real-time execution logs
- Build status (Success, Failure, Unstable)
- Execution time and timestamps
- Any errors or warnings during the build process

</details>

<details>
<summary><b>Upstream and Downstream Projects</b></summary>

### Upstream and Downstream Projects

Jenkins supports project dependencies where one project can trigger another project after completion.

#### Upstream and Downstream Configuration

![Upstream and Downstream Images](jenkins%20screenshot/upstream%20and%20downstream%20images.png)

**Concepts:**
- **Upstream Project**: The project that triggers other projects
- **Downstream Project**: The project that gets triggered by another project
- This creates a build pipeline where projects execute in sequence

#### Configuring Upstream Projects

![Build Configuration for Upstream](jenkins%20screenshot/build%20sonfiguraation%20for%20upstram.png)

**Configuration Steps:**
1. Go to your upstream project configuration
2. Navigate to "Post-build Actions"
3. Add "Build other projects" action
4. Specify the downstream project name
5. Choose trigger conditions (stable build, always, etc.)
6. Save the configuration

</details>

<details>
<summary><b>Best Practices</b></summary>

### Best Practices

- **Version Control**: Always integrate Jenkins with version control systems
- **Build Automation**: Automate builds on code commits
- **Testing**: Include automated tests in your build pipeline
- **Notifications**: Set up email or Slack notifications for build status
- **Security**: Properly configure user permissions and credentials
- **Backup**: Regular backup of Jenkins configuration and jobs

</details>

<details>
<summary><b>Troubleshooting</b></summary>

### Troubleshooting

#### Common Issues:
- **Build Failures**: Check console output for error messages
- **Permission Issues**: Verify user permissions and file access
- **Plugin Conflicts**: Update plugins or check compatibility
- **Resource Issues**: Monitor system resources (CPU, memory, disk space)

</details>

<details>
<summary><b>Future Workflow</b></summary>

### Future Workflow

As you advance in your Jenkins journey, you'll create more complex CI/CD pipelines that integrate multiple stages, automated testing, and deployment processes.

### CI/CD Pipeline Flow

![CI/CD Workflow](jenkins%20screenshot/flow.png)

**Pipeline Stages:**
- **Source Code Management**: Code is committed to version control (Git, SVN)
- **Build**: Jenkins automatically triggers build on code changes
- **Test**: Automated tests run to verify code quality
- **Deploy**: Successful builds are deployed to staging/production environments
- **Monitor**: Track application performance and build metrics

This workflow diagram illustrates how Jenkins orchestrates the entire continuous integration and continuous deployment process, ensuring code quality and rapid delivery.

</details>

<details>
<summary><b>Additional Resources</b></summary>

### Additional Resources

- [Jenkins Official Documentation](https://www.jenkins.io/doc/)
- [Jenkins Plugins Repository](https://plugins.jenkins.io/)
- [Jenkins Community Forums](https://community.jenkins.io/)

</details>

---

## Docker - Containerization

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
<summary><b>� Hands-On: Running Nginx Container</b></summary>

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

This documentation covers the essential concepts for Jenkins CI/CD automation and practical Docker containerization with hands-on examples. Together, these tools form a powerful foundation for modern DevOps practices, enabling continuous integration, continuous delivery, and containerized application deployment.

---

*Last Updated: February 12, 2026*
