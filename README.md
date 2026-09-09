# DevOps Intern Take-Home Assessment

A production-oriented DevOps implementation for the provided web application using Docker, Docker Compose, Traefik, PostgreSQL, LocalStack S3, GitHub Actions, and Docker Hub.

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Technology Stack](#technology-stack)
- [Repository Structure](#repository-structure)
- [Containerization](#containerization)
- [Docker Compose](#docker-compose)
- [PostgreSQL](#postgresql)
- [Object Storage](#object-storage)
- [Health Checks](#health-checks)
- [Traefik Reverse Proxy](#traefik-reverse-proxy)
- [Local Development](#local-development)
- [CI/CD](#cicd)
- [Docker Hub](#docker-hub)
- [Production Deployment](#production-deployment)
- [Rollback](#rollback)
- [Environment Configuration](#environment-configuration)
- [Troubleshooting](#troubleshooting)
- [Git History](#git-history)
- [Verification Checklist](#verification-checklist)

---

## Overview

This repository contains the DevOps implementation for the provided web application.

The application consists of:

- React + TypeScript frontend
- FastAPI backend
- PostgreSQL database
- S3-compatible object storage using LocalStack
- Traefik reverse proxy
- Docker and Docker Compose
- GitHub Actions CI/CD
- Docker Hub image registry

The implementation focuses on:

- Containerization
- Infrastructure configuration
- Service-to-service communication
- Persistent storage
- Object storage integration
- Reverse proxy routing
- Automated Docker image builds
- Docker Hub image publishing
- SHA-based deployments
- Production deployment
- Rollback to a previous known-good version

---

# Architecture

```text
                              Browser
                                 |
                                 | HTTP :80
                                 v
                         +---------------+
                         |    Traefik    |
                         |      :80      |
                         +-------+-------+
                                 |
                  +--------------+--------------+
                  |                             |
                  |                             |
                  v                             v
       app.debyez.localhost           api.debyez.localhost
                  |                             |
                  v                             v
          +---------------+             +---------------+
          |    Frontend   |             |    Backend    |
          | React + Nginx |             |    FastAPI    |
          |      :80      |             |     :8000     |
          +---------------+             +-------+-------+
                                                |
                                  +-------------+-------------+
                                  |             |             |
                                  v             v             v
                           +-----------+  +-----------+  Environment
                           | PostgreSQL|  | LocalStack|  Variables
                           |   :5432   |  |    S3     |
                           +-----------+  |   :4566   |
                                          +-----------+
                                    
<<<<<<< HEAD
## Application overview
=======
#### Application URLs
>>>>>>> ea3d1a967f7e1eeceff23fbf333f53908b8eb151

| Component      | URL                                                                              |
| -------------- | -------------------------------------------------------------------------------- |
| Frontend       | [http://app.debyez.localhost](http://app.debyez.localhost)                       |
| Backend API    | [http://api.debyez.localhost](http://api.debyez.localhost)                       |
| Backend Health | [http://api.debyez.localhost/api/health](http://api.debyez.localhost/api/health) |


## Technology Stack

| Technology         | Purpose                        |
| ------------------ | ------------------------------ |
| Docker             | Containerization               |
| Docker Compose     | Multi-container orchestration  |
| Traefik            | Reverse proxy and routing      |
| React + TypeScript | Frontend                       |
| Nginx              | Frontend production web server |
| FastAPI            | Backend API                    |
| PostgreSQL         | Relational database            |
| LocalStack         | S3-compatible object storage   |
| GitHub Actions     | CI/CD automation               |
| Docker Hub         | Container image registry       |


<<<<<<< HEAD
=======

>>>>>>> ea3d1a967f7e1eeceff23fbf333f53908b8eb151
# Containerization
## Backend Dockerfile

The backend is containerized using Python 3.12 Slim.
<<<<<<< HEAD
## The Backend Container
=======

## The backend container:

>>>>>>> ea3d1a967f7e1eeceff23fbf333f53908b8eb151
    -Installs Python dependencies
    -Copies the application source
    -Runs FastAPI using Uvicorn
    -Listens internally on port 8000
    -Provides a health check through Docker Compose

<<<<<<< HEAD
## File:
=======
# File:
>>>>>>> ea3d1a967f7e1eeceff23fbf333f53908b8eb151

    - backend/Dockerfile

##Backend command:

    - uvicorn app.main:app --host 0.0.0.0 --port 8000

<<<<<<< HEAD
# Repository Structure

├── .github/
│   └── workflows/
│       └── ci-cd.yml
├── backend/
│   ├── app/
│   ├── requirements.txt
│   └── Dockerfile
├── frontend/
│   ├── src/
│   ├── package.json
│   ├── package-lock.json
│   └── Dockerfile
├── docker-compose.yml
├── docker-compose.prod.yml
├── .gitignore
└── README.md

# Docker Containerization
## Backend
##The backend uses Python 3.12 and runs FastAPI with Uvicorn.

        Dockerfile - backend/Dockerfile
The backend listens internally on port 8000.

# Frontend
The frontend uses a multi-stage Docker build.

The application is built using Node.js and the resulting production files are served using Nginx.

        Dockerfile: frontend/Dockerfile
## The API URL is supplied during the frontend Docker build:

        VITE_API_BASE_URL=http://api.debyez.localhost/api        

# DOCKER COMPOSE        
     The local environment is defined in: docker-compose.yml

# Services:

    -Traefik
    -Frontend
    -Backend
    -PostgreSQL
    -LocalStack     
# PostgreSQL Persistence
## PostgreSQL uses the Docker volume: postgres_data

# LocalStack Persistence
    LocalStack uses: localstack_data


# Health Checks
## Backend health endpoint:
    http://api.debyez.localhost/api/health

# Expected response:
{
  "status": "ok",
  "service": "backend"
}

PostgreSQL uses pg_isready for its health check.

The backend container also has a health check using the backend health endpoint.

# S3 Object Storage
### LocalStack provides S3-compatible object storage.

Backend configuration:
        S3_ENDPOINT_URL=http://localstack:4566
        S3_PUBLIC_ENDPOINT_URL=http://localhost:4566
        S3_BUCKET_NAME=devops-files

The application was verified with:

    -File upload
    -File retrieval/open
    -File deletion

# Traefik Reverse Proxy
## Traefik provides hostname-based routing.

Frontend:
        app.debyez.localhost
                |
                v
        Frontend container

Backend :
        api.debyez.localhost
                |
                v
        Backend container

The frontend and backend application ports are not directly exposed to the host.

Traefik exposes HTTP traffic through port 80.

#CI/CD

GitHub Actions workflow:
                    CI/CD
                    .github/workflows/ci-cd.yml

The workflow runs when code is pushed to the main branch.

The pipeline:

    1.Checks out the repository
    2.Sets up Docker Buildx
    3.Logs in to Docker Hub
    4.Builds the backend image
    5.Pushes the backend image
    6.Builds the frontend image
    7.Pushes the frontend image

# Docker Hub repository:

    kabadkhal/devops-assessment

# Docker Image Tagging
    Images are tagged using the Git commit SHA
    Example:
            kabadkhal/devops-assessment:backend-e2a091e...
            kabadkhal/devops-assessment:frontend-e2a091e...

The workflow also publishes latest tags.

SHA-based tags allow a specific application version to be deployed and rolled back.

# GitHub Secrets
    The following GitHub repository secrets are used:
        DOCKERHUB_USERNAME
        DOCKERHUB_TOKEN
        Docker Hub credentials are not stored in the source code.

# Production Deployment
        Production configuration:
             docker-compose.prod.yml
Production uses Docker Hub images directly.

Example:

image: kabadkhal/devops-assessment:backend-<commit-sha>

and:

image: kabadkhal/devops-assessment:frontend-<commit-sha>

The production Compose file does not build application images locally.  

# Production Commands
        Validate the production Compose configuration:
        - docker compose -f docker-compose.prod.yml config

# Start Production
    - docker compose -f docker-compose.prod.yml up -d

# Check production services:
    docker compose -f docker-compose.prod.yml ps

# Production Verification

## Frontend:

    http://app.debyez.localhost

## Backend health:

    http://api.debyez.localhost/api/health

The production deployment was verified by:

    1.Loading the frontend
    2.Checking backend health
    3.Checking PostgreSQL health
    4.Checking S3 connectivity
    5.Uploading a file
    6.Opening/retrieving a file
    7.Deleting a file
    8.Rollback

Production uses SHA-based Docker image tags so a previous known-good version can be restored.

Example:

    Version 1
        |
        v
      SHA A
        |
        v
    Version 2
        |
        v
      SHA B
        |
        v
    Rollback
        |
        v
     SHA A

Rollback was tested successfully.

Version 2 used:

8fc8797ea5ac7bbf68c3801a88f0f1c91e882b22

The production environment was then rolled back to the previous known-good version:

e2a091e6892ebdbf7bc0b88cec06666544366e14

After rollback, the following were verified:

1.Backend healthy
2.Frontend accessible
3.PostgreSQL healthy
4.LocalStack healthy
5.Traefik running
6.API hostname working
7.Frontend hostname working
8.Troubleshooting
9.Check running containers
10.docker compose ps

# Producation
    docker compose -f docker-compose.prod.yml ps

Backend logs
    docker logs devops-backend

# Producation
    docker logs devops-backend-prod

Traefik logs
    docker logs devops-traefik

# Producation
    docker logs devops-traefik-prod

# GitHub Actions

    Open the GitHub repository and select:

    Actions

    Check the latest CI/CD workflow run.

# Docker Hub

Check:

    kabadkhal/devops-assessment

Verify that backend and frontend SHA-tagged images are available.

# Useful Commands

# Stop the development environment:

docker compose down

Start the development environment:

docker compose up -d

View running containers:

docker ps

Check Git status:

git status

View recent commits:

git log --oneline -5
Git History

Important implementation commits include:

Add containerization and local infrastructure
Add GitHub Actions CI/CD workflow
Add production deployment configuration

The project uses multiple meaningful commits to track infrastructure and deployment changes.

Final Verification

The following have been verified:

Backend Docker container works
Frontend production container works
PostgreSQL works
PostgreSQL persistence configured
LocalStack S3 works
File upload works
File retrieval works
File deletion works
Traefik frontend routing works
Traefik backend routing works
GitHub Actions CI/CD works
Docker images are pushed to Docker Hub
Images are tagged using Git commit SHA
Production uses SHA-specific images
Production does not rebuild application images
Production health checks pass
Rollback to a previous known-good SHA works        
    
=======
>>>>>>> ea3d1a967f7e1eeceff23fbf333f53908b8eb151
