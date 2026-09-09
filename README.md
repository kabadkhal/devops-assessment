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
                                    
#### Application URLs

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



#Containerization
##Backend Dockerfile

The backend is containerized using Python 3.12 Slim.

##The backend container:

    -Installs Python dependencies
    -Copies the application source
    -Runs FastAPI using Uvicorn
    -Listens internally on port 8000
    -Provides a health check through Docker Compose

## File:

    - backend/Dockerfile

##Backend command:

    - uvicorn app.main:app --host 0.0.0.0 --port 8000

