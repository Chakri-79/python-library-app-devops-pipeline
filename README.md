# Python Library Management Application — DevOps CI/CD Pipeline

A DevOps project demonstrating automated CI/CD for a Python-based Digital Library Management Application using Jenkins, Docker, Docker Swarm, SonarQube, Trivy, and Docker Hub.

The pipeline automates code checkout, static code analysis, Docker image builds, security scanning, image publishing, and application deployment.

## Project Overview

This project demonstrates how DevOps tools can be integrated to automate the application delivery process.

### Key Features

- GitHub webhook-triggered Jenkins pipeline
- Automated source code checkout
- Static code analysis using SonarQube
- Docker image creation for five application services
- Container image vulnerability scanning using Trivy
- Image publishing to Docker Hub
- Application deployment using Docker Stack and Docker Swarm
- Dedicated documentation and deployment screenshots

## Architecture

The infrastructure consists of four Amazon EC2 instances:

| Instance             | Instance Type  | Role                              |
|----------------------|----------------|-----------------------------------|
| Jenkins Server       | m7i-flex.large | CI/CD pipeline execution          |
| Docker Swarm Manager | m7i-flex.large | Cluster management and deployment |
| Worker 1             | t3.micro       | Container workloads               |
| Worker 2             | t3.micro       | Container workloads               |

The Docker Swarm cluster contains one manager and two worker nodes. The Jenkins server operates separately from the Swarm cluster.

See [Architecture Documentation](docs/architecture.md) for further details.

## Technologies Used

| Category               | Tools                       |
|------------------------|-----------------------------|
| Version Control        | Git, GitHub                 |
| CI/CD                  | Jenkins                     |
| Pipeline Visualization | Jenkins Pipeline Stage View |
| Code Quality           | SonarQube Scanner           |
| Containerization       | Docker                      |
| Orchestration          | Docker Swarm, Docker Stack  |
| Container Security     | Trivy                       |
| Container Registry     | Docker Hub                  |
| Infrastructure         | Amazon EC2                  |
| Application            | Python                      |

## CI/CD Pipeline Stages

The Jenkins pipeline contains six stages.

| Stage    | Description                                       |
|----------|---------------------------------------------------|
| CODE     | Checks out application source code from GitHub    |
| CQA      | Runs static code analysis using SonarQube         |
| BUILD    | Builds five Docker images                         |
| SCAN     | Scans the images using Trivy                      |
| REGISTRY | Pushes images to Docker Hub                       |
| DEPLOY   | Deploys the application stack using Docker Stack  |

A GitHub webhook triggers the Jenkins pipeline when a new commit is pushed.

## Application Services

The application consists of five containerized services.

| Service        | Image Tag     | Port |
|----------------|---------------|------|
| Database       | `dbimage`     | 3306 |
| Authentication | `authimage`   | 5001 |
| Book           | `bookimage`   | 5002 |
| Borrow         | `borrowimage` | 5003 |
| Frontend       | `appimage`    | 5000 |

Docker images are published under:

`chakradhar098/py-lib-app`

The application services use an overlay network named `mynetwork`.

## Deployment

The application is deployed as a Docker Swarm stack named `library`.

```bash
docker stack deploy -c compose.yml library
```