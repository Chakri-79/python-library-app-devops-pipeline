# Python Library Management Application — Architecture

## 1. Project Overview

This project demonstrates a CI/CD pipeline for a Python-based Digital Library Management Application.

The pipeline automates source code checkout, static code analysis, Docker image creation, container image security scanning, image publishing, and application deployment using Docker Swarm.

The application is hosted in a separate GitHub repository, while this repository contains the DevOps configuration and project documentation.

## 2. Infrastructure

The project uses four Amazon EC2 instances.

| Instance             | Instance Type  | Responsibility                                        |
|----------------------|----------------|-------------------------------------------------------|
| Jenkins Server       | m7i-flex.large | Runs Jenkins and orchestrates the CI/CD pipeline      |
| Docker Swarm Manager | m7i-flex.large | Manages the Swarm cluster and application deployments |
| Worker 1             | t3.micro       | Runs containerized application services               |
| Worker 2             | t3.micro       | Runs containerized application services               |

### Docker Swarm Cluster

The Docker Swarm cluster consists of three nodes:

- One manager node
- Two worker nodes

The manager controls the cluster and schedules services across the available nodes. The worker nodes run the application containers assigned to them.

The Jenkins server is separate from the three-node Docker Swarm cluster.

## 3. Technologies Used

- Git and GitHub
- Jenkins
- Jenkins Pipeline Stage View
- SonarQube Scanner
- Docker
- Docker Pipeline plugin
- Docker Swarm
- Docker Compose
- Trivy
- Docker Hub
- Amazon EC2
- Linux

## 4. CI/CD Pipeline

The pipeline consists of six stages.

### Stage 1: CODE

Checks out the Python application source code from GitHub.

Application repository:
https://github.com/Chakri-79/python-code-library-app

### Stage 2: CQA

Runs static code analysis using SonarQube Scanner.

The analysis helps identify code quality issues and security-related findings.

### Stage 3: BUILD

Builds five Docker images:

| Component               | Docker Image Tag |
|-------------------------|------------------|
| Database                | dbimage          |
| Authentication service  | authimage        |
| Book service            | bookimage        |
| Borrow service          | borrowimage      |
| Frontend application    | appimage         |

The images are built using their respective Dockerfiles.

### Stage 4: SCAN

Uses Trivy to scan the Docker images for known vulnerabilities.

The scan results must be reviewed to identify vulnerabilities requiring remediation.

### Stage 5: REGISTRY

Pushes the Docker images to Docker Hub.

Docker Hub repository:
chakradhar098/py-lib-app

### Stage 6: DEPLOY

Deploys the application using Docker Stack and the Compose configuration.

The Docker Swarm stack name is:

library

Deployment command:

```bash
docker stack deploy -c compose.yml library
```