
## RoboShop Components CI/CD

This repository contains the **Kubernetes Manifests** and **CI/CD Configurations** for the RoboShop microservices application. It follows a GitOps pattern to manage deployments across different environments.

## 📂 Repository Structure

The repository is organized into functional areas for easy management:

### 🚀 Component Manifests (`/robo-component-manifests`)
Contains the application-level Kubernetes manifests for each microservice. Each service has a `-ci` and `-deploy` directory:
* **[Service]-ci**: Configurations for Continuous Integration.
* **[Service]-deploy**: Kubernetes deployment, service, and HPA manifests.
* **Components**: `cart`, `catalogue`, `frontend`, `payment`, `shipping`, `user`.

### 🗄️ Database Manifests (`/robo-db-manifests`)
Stateful components required for the application to function:
* **MongoDB**: NoSQL database for product and user data.
* **MySQL**: Relational database for shipping information.
* **RabbitMQ**: Message broker for payment processing.
* **Redis**: In-memory cache for session management.

## 🛠️ CI/CD Workflow

1.  **Code Commit**: Developers push code to application-specific repositories.
2.  **Jenkins Pipeline**: Triggers a build using the [Jenkins Shared Library](https://github.com/venkatesh-thomm/jenkins-shared-library).
3.  **Artifact Creation**: Docker images are built and pushed to a container registry.
4.  **Manifest Update**: The CI pipeline updates the image tags in the `*-deploy` folders in this repo.
5.  **Deployment**: Kubernetes clusters sync these changes automatically via ArgoCD.

## 📋 Prerequisites

* **Kubernetes Cluster**: EKS, GKE, or Minikube.
* **Jenkins Server**: Configured with the shared library.
* **Tools**: `kubectl`, `helm`, and `kustomize`.
```
---

```
###  2. Jenkins Shared Library README.md

**Location:** (In your separate Shared Library repo)

# Jenkins Shared Library for RoboShop

This library provides reusable Groovy scripts and pipeline templates to standardize CI/CD across all RoboShop microservices. It reduces code duplication and ensures best practices for security and deployment.

## 🚀 Features

* **Standardized Build Stages**: Checkout, Unit Tests, Code Analysis (SonarQube), and Docker Build.
* **Environment Management**: Dynamic tagging for `dev`, `stage`, and `prod`.
* **Security Scanning**: Integrated vulnerability scanning for images.
* **GitOps Integration**: Automated manifest updates in the deployment repository.

## 🛠️ Usage

To use this library in your Jenkinsfile, add the following at the top:

```groovy
@Library('jenkins-shared-library') _

// Example for a Node.js component
roboshopPipeline({
    component = 'catalogue'
    language  = 'nodejs'
    appVersion = '1.0.0'
})
```

## 📂 Directory Structure

* **`vars/`**: Contains the global variables and pipeline templates (e.g., `roboshopPipeline.groovy`).
* **`src/`**: Helper classes and logic for complex operations.
* **`resources/`**: Static configuration files or JSON templates used during the build.

## ⚙️ Requirements

* **Jenkins Plugins**: Pipeline, Git, Docker, SonarQube Scanner.
* **Credentials**: Defined in Jenkins for GitHub, DockerHub, and AWS/Kubernetes.

---
