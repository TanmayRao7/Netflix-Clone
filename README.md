# DevSecOps Project - Netflix Clone Application

This project demonstrates the implementation of DevSecOps practices using a Netflix Clone application. The following tools and technologies are employed for CI/CD, security scanning, and deployment automation.

## Tools & Technologies Used

- **CI/CD Tool**: Jenkins
- **Security Tools**: 
  - Trivy (Filesystem & Image Scanning)
  - SonarQube (Code Quality & Security Analysis)
  - OWASP Dependency Check (Vulnerability Detection)
- **Deployment**: Docker, Kubernetes (Minikube)
- **Monitoring**: Prometheus & Grafana Dashboards
- **Version Control System**: Git and GitHub

## CI/CD Pipeline with Jenkins

This project utilizes a Jenkins pipeline to automate the build, test, and deployment processes. The pipeline integrates with security and monitoring tools to ensure a robust and secure deployment workflow.

### Key Pipeline Stages:
1. **Build & Test**: Automated builds using Jenkins with unit testing.
2. **Code Quality & Security Analysis**: SonarQube is integrated to check code quality, detect bugs, and identify security vulnerabilities.
3. **Container Scanning**: Trivy is used for both filesystem and image scans to detect security issues in dependencies and container images.
4. **Vulnerability Detection**: OWASP Dependency Check ensures no known vulnerabilities exist in the application's dependencies.
5. **Deployment**: The application is deployed in Docker containers, orchestrated using Kubernetes on Minikube.

## Security Tools Integration

### SonarQube Analysis
SonarQube is integrated into the Jenkins pipeline to provide continuous inspection of code quality and security. It checks for code bugs, code smells, and security vulnerabilities.

### Trivy File System & Image Scans
Trivy is used to perform filesystem scans to detect security issues in the project's dependencies. Additionally, image scanning ensures that Docker images are free from vulnerabilities before deployment.

### OWASP Dependency Check
OWASP Dependency Check is used to analyze the project dependencies for known vulnerabilities. It integrates into the pipeline to halt deployment if critical vulnerabilities are found.

## Monitoring & Visualization

### Prometheus
Prometheus is configured to monitor the application and the underlying infrastructure. It collects metrics and triggers alerts based on predefined thresholds.

### Grafana Dashboards
Grafana is used for visualizing data collected by Prometheus. The dashboards provide insights into the application's performance, resource usage, and any security events detected.

## Application Demo

A fully functional demo of the Netflix Clone application is included, showcasing the final deployment and all integrated security and monitoring tools.

---

