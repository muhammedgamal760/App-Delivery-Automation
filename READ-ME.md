# Jenkins-Zero-To-Hero: Java-Maven-SonarQube-ArgoCD-Helm-K8s

![CI/CD Pipeline Architecture](https://user-images.githubusercontent.com/43399466/228301952-abc02ca2-9942-4a67-829317647b6f98b.png)

## 🚀 Project Overview

This repository demonstrates an end-to-end CI/CD pipeline for a Java application using Jenkins, SonarQube, Docker, Kubernetes (k3d), and ArgoCD. The pipeline implements modern DevOps practices including:

- Continuous Integration with Jenkins
- Code Quality Analysis with SonarQube
- Containerization with Docker
- GitOps-based Continuous Deployment with ArgoCD
- Container Orchestration with Kubernetes (k3d)
- High Availability cluster setup

## 🏗️ Architecture

The implementation follows a GitOps approach with clear separation between CI and CD:

1. **CI Pipeline (Jenkins)**:
   - Triggered by webhook on code changes
   - Builds Java application with Maven
   - Performs code quality analysis with SonarQube
   - Runs automated tests
   - Builds and pushes Docker image if tests and quality gates pass
   
2. **CD Pipeline (ArgoCD)**:
   - Monitors manifest repository
   - Automatically syncs Kubernetes resources
   - Ensures desired state in the cluster

## 🛠️ Environment Setup

### Local Environment Components

- **Jenkins**: Installed locally on the host machine
- **SonarQube**: Running as a Docker container
- **Jenkins Agent**: Custom Docker image that shares host network to connect with SonarQube
- **K3d Cluster**: 3-node Kubernetes cluster serving as both control plane and worker nodes
- **ArgoCD**: Installed as an operator within the K3d cluster

### High Availability Features

- Multiple K3d nodes (3) configured for high availability
- Nodes serve as both masters and workers
- Resilient to single node failures

## 🚀 Getting Started

### Prerequisites

- Docker and Docker Compose
- Java Development Kit (JDK)
- Maven
- kubectl
- k3d
- helm

### Setup Instructions

1. **Clone the repository**:
   ```bash
   git clone https://github.com/yourusername/jenkins-zero-to-hero.git
   cd jenkins-zero-to-hero
   ```

2. **Set up Jenkins**:
   - Install Jenkins on your local machine following the [official documentation](https://www.jenkins.io/doc/book/installing/)
   - Configure Jenkins with required plugins (Git, Maven, Docker, etc.)

3. **Set up SonarQube container**:
   ```bash
   docker run -d --name sonarqube -p 9000:9000 sonarqube:latest
   ```

4. **Build the Jenkins agent**:
   ```bash
   cd agent
   docker build -t jenkins-agent .
   ```

5. **Set up K3d cluster**:
   ```bash
   k3d cluster create mycluster --servers 3 --agents 0 -p "8080:80@loadbalancer"
   ```

6. **Install ArgoCD**:
   ```bash
   kubectl create namespace argocd
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```

## 📋 Pipeline Configuration

### Jenkins Pipeline Configuration

1. Create a new Jenkins pipeline job
2. Configure it to use the Jenkinsfile from this repository
3. Set up webhooks in your Git repository to trigger the pipeline on code changes

### SonarQube Configuration

1. Access SonarQube at http://localhost:9000
2. Create a new project and generate an authentication token
3. Configure Jenkins with the SonarQube token

### ArgoCD Configuration

1. Access ArgoCD UI (port-forward or through ingress)
2. Add your manifest repository
3. Configure automatic sync policies

## 📂 Repository Structure

```
├── agent/                  # Jenkins agent Dockerfile and configs
├── java-maven-sonar-argocd-helm-k8s/
│   ├── spring-boot-app/    # Java application source code
│   ├── spring-boot-app-manifests/
│   │   ├── deployment.yml  # Kubernetes deployment manifests
│   │   └── service.yml     # Kubernetes service manifests
├── Jenkinsfile             # Jenkins pipeline definition
└── README.md               # Project documentation
```

## 🔄 CI/CD Workflow

1. Developer pushes code to the Git repository
2. Jenkins pipeline is triggered via webhook
3. Jenkins builds the Java application using Maven
4. SonarQube analyzes the code for quality issues
5. If quality gates pass, Jenkins builds a Docker image
6. Docker image is pushed to Docker Hub
7. Jenkins updates the image tag in the manifests repository
8. ArgoCD detects changes in the manifests repository
9. ArgoCD synchronizes the application state with the K3d cluster

## 📝 Notes

- The Jenkins agent is configured to share the host network to communicate with SonarQube
- K3d is used as a lightweight Kubernetes distribution for local development
- ArgoCD implements the GitOps principles for the CD part of the pipeline

## 🔒 Security Considerations

- Sensitive credentials are stored in Jenkins credentials manager
- SonarQube authentication is managed via tokens
- Network security between components is maintained through proper configuration

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.
