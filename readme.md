# 🚀 Dockerized Node.js Application with Nginx & Portainer

A production-ready Docker Compose setup featuring a Node.js Express backend, Nginx reverse proxy, and Portainer container management dashboard.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Architecture](#architecture)
- [Installation & Setup](#installation--setup)
- [Quick Start](#quick-start)
- [Accessing Services](#accessing-services)
- [Docker Commands Reference](#docker-commands-reference)
- [Troubleshooting](#troubleshooting)
- [License](#license)

## Overview

This project demonstrates a containerized multi-service application architecture using Docker and Docker Compose. It provides an isolated, scalable environment with:

- **Express.js Backend**: RESTful API server running on port 3000
- **Nginx Reverse Proxy**: HTTP traffic handler on port 80
- **Portainer GUI**: Docker container management interface on port 9443

Perfect for development, testing, and small-scale production deployments.

## Features

✅ **Multi-container orchestration** via Docker Compose  
✅ **Nginx reverse proxy** for HTTP request routing  
✅ **Auto-restart policy** for reliable uptime  
✅ **Portainer dashboard** for easy container monitoring  
✅ **Alpine Linux** for minimal image sizes  
✅ **Hot-reload capability** for code updates  
✅ **Persistent volumes** for Portainer data  

## Prerequisites

Ensure the following are installed on your system:

- **Docker**: [Install Docker](https://docs.docker.com/get-docker/)
- **Docker Compose**: [Install Docker Compose](https://docs.docker.com/compose/install/)
- **Git** (optional, for cloning)
- **Linux/macOS/Windows** with Docker Desktop enabled

**Verify installation:**

```bash
docker --version
docker-compose --version
```

## Project Structure

```
.
├── Dockerfile                 # Node.js application container configuration
├── docker-compose.yml         # Multi-container orchestration file
├── index.js                   # Express.js entry point
├── package.json               # Node.js dependencies
├── package-lock.json          # Dependency lock file (auto-generated)
├── nginx/
│   ├── default.conf           # Nginx reverse proxy configuration
│   └── Dockerfile             # Nginx container setup (if custom build)
└── README.md                  # Project documentation
```

## Architecture

```
User Request → Nginx (Port 80) → Node.js App (Port 3000)
                                     ↓
                            Express.js Server
                                     ↓
                            Response → Client
```

**Service Details:**

| Service | Port | Purpose | Status |
|---------|------|---------|--------|
| **Nginx** | 80 | HTTP entry point, reverse proxy | External |
| **Node.js** | 3000 | Backend API, internal only | Internal |
| **Portainer** | 9443 | Container management UI | External |

## Installation & Setup

### 1. Clone the Repository

```bash
git clone <your-repo-url>
cd Docker
```

### 2. Verify Files

Ensure all required files are present:

```bash
ls -la
# Should see: Dockerfile, docker-compose.yml, index.js, package.json, nginx/
```

### 3. Review Configuration

Check `docker-compose.yml` and `nginx/default.conf` to ensure settings match your requirements.

## Quick Start

### Start All Services

```bash
sudo docker-compose up --build -d
```

This command:
- Downloads required Docker images
- Builds the Node.js application image
- Starts all containers in detached mode

### Stop All Services

```bash
sudo docker-compose down
```

This preserves your data volumes and configuration.

### Restart Services

```bash
sudo docker-compose restart
```

### Apply Code Updates

After modifying `index.js` or `package.json`:

```bash
sudo docker-compose up -d
# Or rebuild if dependencies changed:
sudo docker-compose up --build -d
```

## Accessing Services

Once containers are running, access the following endpoints:

### Node.js Application
```
http://localhost
```
Displays: `Hello World from Node.js!`

### Portainer GUI
```
https://localhost:9443
```

**First Access:**
1. You'll see a security warning (self-signed certificate)
2. Click **Advanced** → **Proceed to localhost (unsafe)**
3. Set your admin password
4. Login and manage containers

## Docker Commands Reference

### View Container Status

```bash
sudo docker-compose ps
```

### View Real-time Logs

```bash
# All services
sudo docker-compose logs -f

# Specific service (node_app)
sudo docker-compose logs -f node_app

# Specific service (nginx)
sudo docker-compose logs -f nginx_server

# View last 50 lines
sudo docker-compose logs --tail=50
```

### Execute Commands in Container

```bash
# Access Node.js container shell
sudo docker-compose exec node_app sh

# Run npm command
sudo docker-compose exec node_app npm list
```

### View Resource Usage

```bash
docker stats
```

### Remove Volumes & Containers

```bash
# Warning: This removes all data!
sudo docker-compose down -v
```

### View Docker Images

```bash
docker images
```

### Clean Up Unused Resources

```bash
docker system prune -a
```

## Troubleshooting

### Port Already in Use

```bash
# Find process using port 80
sudo lsof -i :80

# Kill process
sudo kill -9 <PID>
```

### Containers Fail to Start

```bash
# Check logs
sudo docker-compose logs

# Rebuild images
sudo docker-compose down
sudo docker-compose up --build -d
```

### Portainer Connection Issues

- Ensure Docker socket is accessible: `/var/run/docker.sock`
- Restart Portainer: `sudo docker-compose restart portainer_gui`

### Changes Not Reflected

```bash
# Rebuild images if dependencies changed
sudo docker-compose up --build -d
```

### Permission Denied Errors

```bash
# Add user to docker group (Linux)
sudo usermod -aG docker $USER
newgrp docker
```

## Configuration Files

### `docker-compose.yml`
Defines three services:
- **node_app**: Builds and runs the Express.js application
- **nginx_server**: Routes HTTP traffic to the Node.js backend
- **portainer_gui**: Provides container management interface

### `nginx/default.conf`
Configures Nginx reverse proxy with proper headers and WebSocket support.

### `Dockerfile`
- Base image: `node:20-alpine`
- Installs dependencies and runs the application

### `index.js`
Simple Express.js server returning "Hello World from Node.js!"

## Environment Variables

Currently not used. To add environment support, modify `docker-compose.yml`:

```yaml
services:
  node_app:
    environment:
      - NODE_ENV=production
```

## Production Considerations

For production deployment:

- [ ] Add environment-specific configurations
- [ ] Implement SSL/TLS certificates for Nginx
- [ ] Set up health checks in `docker-compose.yml`
- [ ] Configure logging drivers
- [ ] Use external secrets management
- [ ] Add resource limits and reservations
- [ ] Implement container image scanning
- [ ] Set up monitoring and alerting

## License

This project is open source. Modify and use as needed for your applications.