# Docker Node.js Development Environment

## Overview

This project demonstrates how to use Docker to create a Node.js development environment without installing Node.js or npm directly on the host machine. This approach is particularly useful when working with EC2 instances or any environment where you want to maintain a clean host system.

## Benefits

- No need to install nvm or Node.js on the host machine
- Consistent development environment across different machines
- Easy to set up and use
- Changes made in the container are reflected on the host through volume mounting

## Setup Process

### 1. Create a Dockerfile

Create a simple Dockerfile that uses Node.js Alpine image and sets npm as the entry point:

```dockerfile
FROM node:14-alpine

WORKDIR /app

ENTRYPOINT ["npm"]
```

### 2. Build the Docker Image

Build the Docker image with a tag for easy reference:

```bash
docker build -t node-util .
```

### 3. Use the Docker Container with Volume Mounting

Run commands using the container while mounting the current directory to persist changes:

```bash
# Initialize npm project (creates package.json)
docker run -it -v $(pwd):/app node-util init

# Install dependencies
docker run -it -v $(pwd):/app node-util install
```

The `-v $(pwd):/app` flag mounts the current directory to the `/app` directory in the container, ensuring that any files created or modified in the container are also available on the host.

## Container Management

### View Running Containers

To see all containers (including stopped ones):

```bash
docker ps -a
```

### Interact with a Running Container

To enter a running container:

```bash
docker exec -it <container_id> /bin/bash
```

### Run npm Commands in a Container

You can run various npm commands directly:

```bash
docker exec -it <container_id> npm init -y
```

## Common Node.js Package Installation

Here are some common packages you might want to install:

```bash
# Web framework
docker run -it -v $(pwd):/app node-util install express --save

# Common middleware and utilities
docker run -it -v $(pwd):/app node-util install body-parser dotenv morgan cors --save

# Database connectivity
docker run -it -v $(pwd):/app node-util install mongoose --save

# Development tools
docker run -it -v $(pwd):/app node-util install nodemon --save-dev

# Authentication
docker run -it -v $(pwd):/app node-util install jsonwebtoken bcryptjs --save

# File uploads
docker run -it -v $(pwd):/app node-util install multer multer-gridfs-storage --save

# Real-time communication
docker run -it -v $(pwd):/app node-util install socket.io socket.io-client --save
```

## Notes

- The container stops after each command completes because the ENTRYPOINT is set to npm
- To keep a container running, you would need a different approach with a CMD that doesn't exit
- All files created in the mounted directory will have the same permissions as if they were created directly on the host

