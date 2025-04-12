# SEFA + RAMSES Docker Compose Setup

This document provides instructions for setting up and running the SEFA + RAMSES system using Docker Compose.

## Prerequisites

- Docker and Docker Compose installed
- GitHub OAuth token
- GitHub repository URL for the config server

## Environment Variables Setup

Before running the system, you need to set the following environment variables:

```bash
# Set your architecture (arm64 or amd64)
export ARCH=amd64  # or arm64

# Set your GitHub credentials
export GITHUB_OAUTH=your_github_oauth_token
export GITHUB_REPOSITORY_URL=your_config_repo_url
```

## Running the System

### 1. Starting All Services

To start all services in detached mode:

```bash
docker-compose up -d
```

This will start all services except the load generator, including:
- MySQL database
- SEFA services (Eureka, Config Server, etc.)
- RAMSES services
- Probe and Actuators


### 2. Starting Specific Services

If you want to start only specific services, you can specify them:

```bash
# Start only core services
docker-compose up -d sefa-eureka sefa-configserver mysql
```

### 3. Starting Load Generator Separately

To start the load generator:

```bash
docker-compose up -d sefa-load-generator
```

### 4. Managing Services

- To view logs:
  ```bash
  docker-compose logs -f [service-name]
  ```

- To stop all services:
  ```bash
  docker-compose down
  ```

- To stop specific services:
  ```bash
  docker-compose stop [service-name]
  ```

## Advantages of Docker Compose Setup

The Docker Compose approach offers several advantages over the shell script:

1. **Better Dependency Management**
   - Services start in the correct order
   - No need for manual sleep commands
   - Proper service health checks

2. **Easier Scaling**
   - Simple to scale services up or down
   - Better resource management

3. **Improved Container Lifecycle**
   - Better container lifecycle management
   - Easier to restart or update services

4. **Maintainability**
   - Configuration is centralized in one file
   - Easier to modify and maintain
   - Better documentation of service relationships

5. **Network Management**
   - Automatic network creation
   - Better service isolation
   - Easier service communication

## Service Dependencies

The services are organized with proper dependencies:
- MySQL is started first
- Eureka server depends on MySQL
- Config server depends on Eureka
- All other services depend on both Config server and Eureka

## Network Configuration

- A bridge network called `ramses-sas-net` is automatically created
- All services are connected to this network
- Services can communicate with each other using their service names as hostnames 