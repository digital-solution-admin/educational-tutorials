# Docker Fundamentals: A Practical Guide

Docker has revolutionized how developers build, ship, and run applications. This guide will walk you through Docker fundamentals with practical examples to get you up and running quickly.

## Table of Contents

1. [Introduction to Docker](#introduction-to-docker)
2. [Installation](#installation)
3. [Core Concepts](#core-concepts)
4. [Working with Docker Images](#working-with-docker-images)
5. [Working with Docker Containers](#working-with-docker-containers)
6. [Docker Networking](#docker-networking)
7. [Docker Volumes](#docker-volumes)
8. [Dockerfile Best Practices](#dockerfile-best-practices)
9. [Docker Compose](#docker-compose)
10. [Docker in Production](#docker-in-production)
11. [Common Docker Commands Reference](#common-docker-commands-reference)

## Introduction to Docker

Docker is a platform that enables developers to build, deploy, and run applications in containers. Containers are lightweight, portable, and self-sufficient units that can run virtually anywhere.

### Benefits of Docker

- **Consistency**: Run the same application with the same configuration across multiple environments
- **Isolation**: Applications run in their own environment without interfering with each other
- **Portability**: Build once, run anywhere (local, staging, production)
- **Efficiency**: Containers share the host OS kernel, making them more lightweight than VMs
- **Scalability**: Easily scale applications horizontally by adding more container instances

## Installation

### Windows
1. Install [Docker Desktop for Windows](https://docs.docker.com/desktop/install/windows-install/)
2. Make sure WSL 2 is enabled for better performance

### macOS
1. Install [Docker Desktop for Mac](https://docs.docker.com/desktop/install/mac-install/)

### Linux
1. Install Docker Engine:
   ```bash
   # Ubuntu/Debian
   sudo apt-get update
   sudo apt-get install docker-ce docker-ce-cli containerd.io
   
   # CentOS/RHEL
   sudo yum install docker-ce docker-ce-cli containerd.io
   ```

2. Start Docker service:
   ```bash
   sudo systemctl start docker
   sudo systemctl enable docker
   ```

3. Add your user to the docker group to run Docker without sudo:
   ```bash
   sudo usermod -aG docker $USER
   ```

## Core Concepts

### Docker Architecture

Docker uses a client-server architecture:
- **Docker Client**: The command-line tool that users interact with
- **Docker Daemon (Server)**: The background service that manages Docker objects
- **Docker Registry**: A storage and distribution system for Docker images (e.g., Docker Hub)

### Key Components

- **Images**: Read-only templates used to create containers
- **Containers**: Running instances of Docker images
- **Dockerfile**: A text file with instructions to build a Docker image
- **Volumes**: Persistent data storage for containers
- **Networks**: Communication between containers or with the outside world

## Working with Docker Images

### Pulling Images from Docker Hub

```bash
# Pull the latest version of an image
docker pull nginx

# Pull a specific version of an image
docker pull node:16

# Pull an image from a different registry
docker pull mcr.microsoft.com/dotnet/sdk:6.0
```

### Listing Local Images

```bash
# List all images
docker images

# List images with specific format
docker images --format "{{.Repository}}:{{.Tag}}"
```

### Building Images with Dockerfile

Create a file named `Dockerfile`:

```dockerfile
FROM node:16

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

Build the image:

```bash
# Build with a tag
docker build -t myapp:1.0 .

# Build with no cache
docker build --no-cache -t myapp:1.0 .
```

### Dockerfile Instructions Explained

- **FROM**: Base image to start from
- **WORKDIR**: Sets the working directory inside the container
- **COPY/ADD**: Copies files from host to container
- **RUN**: Executes commands during the build
- **EXPOSE**: Documents which ports the container listens on
- **ENV**: Sets environment variables
- **CMD/ENTRYPOINT**: Specifies what to run when the container starts

### Managing Images

```bash
# Remove an image
docker rmi nginx

# Remove all unused images
docker image prune

# Remove all images
docker image prune -a

# Tag an image
docker tag myapp:1.0 myregistry.com/myapp:1.0
```

## Working with Docker Containers

### Running Containers

```bash
# Run a container in the foreground
docker run nginx

# Run a container in the background (detached mode)
docker run -d nginx

# Run with a specific name
docker run -d --name my-nginx nginx

# Run with port mapping
docker run -d -p 8080:80 nginx

# Run with environment variables
docker run -d -e DB_HOST=db -e DB_PASSWORD=secret mysql
```

### Managing Running Containers

```bash
# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# Stop a container
docker stop container_id

# Start a stopped container
docker start container_id

# Restart a container
docker restart container_id

# Remove a container
docker rm container_id

# Remove a running container forcefully
docker rm -f container_id

# Remove all stopped containers
docker container prune
```

### Interacting with Containers

```bash
# Execute commands in a running container
docker exec -it container_id bash

# View container logs
docker logs container_id

# Follow container logs
docker logs -f container_id

# View container stats (CPU, memory)
docker stats container_id
```

## Docker Networking

### Network Types

- **bridge**: Default network for containers on a single host
- **host**: Container shares the host's network namespace
- **none**: Container has no network access
- **overlay**: Connect containers across multiple Docker hosts
- **macvlan**: Assign a MAC address to a container

### Managing Networks

```bash
# List networks
docker network ls

# Create a network
docker network create my-network

# Inspect a network
docker network inspect my-network

# Connect a container to a network
docker network connect my-network container_id

# Disconnect a container from a network
docker network disconnect my-network container_id

# Remove a network
docker network rm my-network
```

### Container Communication Example

Create a custom network and run containers on it:

```bash
# Create network
docker network create my-app-network

# Run a database container
docker run -d --name db --network my-app-network mongo

# Run an API container that connects to the database
docker run -d --name api --network my-app-network -e DB_HOST=db my-api-image
```

The API container can now communicate with the database container using the hostname `db`.

## Docker Volumes

### Volume Types

- **Named volumes**: Managed by Docker, easier to back up and migrate
- **Bind mounts**: Maps a host directory to a container directory
- **tmpfs mounts**: Stored in host memory only, not on disk

### Managing Volumes

```bash
# Create a named volume
docker volume create my-data

# List volumes
docker volume ls

# Inspect a volume
docker volume inspect my-data

# Remove a volume
docker volume rm my-data

# Remove all unused volumes
docker volume prune
```

### Using Volumes with Containers

```bash
# Use a named volume
docker run -d -v my-data:/data nginx

# Use a bind mount
docker run -d -v $(pwd):/app node

# Read-only bind mount
docker run -d -v $(pwd):/app:ro node
```

### Practical Volume Examples

1. **Database Persistence**
   ```bash
   docker run -d --name postgres -v postgres-data:/var/lib/postgresql/data postgres
   ```

2. **Development with Live Reload**
   ```bash
   docker run -d --name node-app -v $(pwd):/app -p 3000:3000 node-dev-image
   ```

## Dockerfile Best Practices

### Optimize for Caching

```dockerfile
# Good: Dependencies change less frequently than code
FROM node:16
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
CMD ["npm", "start"]
```

### Multi-stage Builds

```dockerfile
# Build stage
FROM node:16 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### Reduce Image Size

1. **Use smaller base images**
   ```dockerfile
   FROM node:16-alpine  # Instead of node:16
   ```

2. **Clean up in the same layer**
   ```dockerfile
   RUN apt-get update && \
       apt-get install -y some-package && \
       apt-get clean && \
       rm -rf /var/lib/apt/lists/*
   ```

3. **Remove unnecessary files**
   ```dockerfile
   RUN npm install && \
       npm cache clean --force
   ```

### Security Best Practices

1. **Don't run as root**
   ```dockerfile
   RUN useradd -ms /bin/bash appuser
   USER appuser
   ```

2. **Don't store secrets in images**
   ```dockerfile
   # Use build args for build-time secrets
   ARG NPM_TOKEN
   RUN echo "//registry.npmjs.org/:_authToken=${NPM_TOKEN}" > .npmrc && \
       npm install && \
       rm .npmrc
   ```

3. **Use specific versions**
   ```dockerfile
   FROM node:16.15.1-alpine3.16
   ```

## Docker Compose

Docker Compose is a tool for defining and running multi-container Docker applications.

### Basic docker-compose.yml Example

```yaml
version: '3.8'

services:
  web:
    build: ./web
    ports:
      - "8080:80"
    depends_on:
      - api
    environment:
      - API_URL=http://api:3000

  api:
    build: ./api
    ports:
      - "3000:3000"
    depends_on:
      - db
    environment:
      - DB_HOST=db
      - DB_USER=postgres
      - DB_PASSWORD=secret

  db:
    image: postgres:14
    volumes:
      - db-data:/var/lib/postgresql/data
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=secret

volumes:
  db-data:
```

### Common Docker Compose Commands

```bash
# Start services
docker-compose up

# Start in detached mode
docker-compose up -d

# Stop services
docker-compose down

# Stop and remove volumes
docker-compose down -v

# View logs
docker-compose logs

# Follow logs for specific service
docker-compose logs -f api

# Rebuild services
docker-compose up -d --build

# Scale a service
docker-compose up -d --scale api=3
```

### Docker Compose for Development

```yaml
version: '3.8'

services:
  web:
    build: 
      context: ./web
      dockerfile: Dockerfile.dev
    ports:
      - "8080:80"
    volumes:
      - ./web:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development
```

## Docker in Production

### Container Orchestration

For production environments, consider using orchestration platforms:

- **Kubernetes**: Industry standard for container orchestration
- **Docker Swarm**: Built into Docker, simpler than Kubernetes
- **Amazon ECS/EKS**: AWS container services
- **Azure AKS**: Azure's managed Kubernetes service
- **Google GKE**: Google's managed Kubernetes service

### Production Considerations

1. **Use Health Checks**
   ```dockerfile
   HEALTHCHECK --interval=30s --timeout=3s \
     CMD curl -f http://localhost/ || exit 1
   ```

2. **Implement Logging**
   ```bash
   # Use logging drivers
   docker run --log-driver=fluentd nginx
   ```

3. **Resource Constraints**
   ```bash
   # Limit CPU and memory
   docker run -d --cpus=0.5 --memory=512m nginx
   ```

4. **Container Monitoring**
   - Prometheus and Grafana
   - Datadog
   - New Relic
   - Elastic Stack

5. **CI/CD Integration**
   - Automated builds on code changes
   - Automated testing of containers
   - Automated deployment

## Common Docker Commands Reference

### Image Commands
```bash
docker build -t image-name:tag .             # Build an image
docker pull image-name:tag                   # Pull an image
docker push image-name:tag                   # Push an image
docker images                                # List images
docker rmi image-name:tag                    # Remove an image
docker history image-name:tag                # Show image history
docker inspect image-name:tag                # Inspect an image
```

### Container Commands
```bash
docker run -d -p 8080:80 image-name          # Run a container
docker ps                                    # List running containers
docker ps -a                                 # List all containers
docker stop container-id                     # Stop a container
docker start container-id                    # Start a container
docker restart container-id                  # Restart a container
docker rm container-id                       # Remove a container
docker logs container-id                     # View logs
docker exec -it container-id bash            # Enter a container
docker inspect container-id                  # Inspect a container
docker stats                                 # Show container stats
```

### Volume Commands
```bash
docker volume create volume-name             # Create a volume
docker volume ls                             # List volumes
docker volume inspect volume-name            # Inspect a volume
docker volume rm volume-name                 # Remove a volume
docker volume prune                          # Remove unused volumes
```

### Network Commands
```bash
docker network create network-name           # Create a network
docker network ls                            # List networks
docker network inspect network-name          # Inspect a network
docker network connect network-name container-id # Connect container to network
docker network disconnect network-name container-id # Disconnect container
docker network rm network-name               # Remove a network
```

### System Commands
```bash
docker info                                  # Display system info
docker version                               # Show Docker version
docker system df                             # Show disk usage
docker system prune                          # Remove unused data
docker system prune -a                       # Remove all unused data
```

## Conclusion

Docker has become an essential tool in modern software development. By containerizing applications, developers can ensure consistency across environments, improve deployment efficiency, and simplify dependency management.

This guide covers the fundamentals, but Docker's ecosystem is vast and continuously evolving. Keep exploring and experimenting to fully leverage the power of containerization in your projects.

## Additional Resources

- [Official Docker Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/) - Repository of Docker images
- [Play with Docker](https://labs.play-with-docker.com/) - Interactive Docker playground
- [Docker GitHub Repository](https://github.com/docker/docker-ce)
- [Docker Roadmap](https://github.com/docker/roadmap)
