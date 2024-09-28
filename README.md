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

![image](https://github.com/user-attachments/assets/c2e10c0e-2232-4be8-b2f2-d5cb0684d26b)


### Key Pipeline Stages:
1. **Build & Test**: Automated builds using Jenkins with unit testing.
2. **Code Quality & Security Analysis**: SonarQube is integrated to check code quality, detect bugs, and identify security vulnerabilities.
3. **Container Scanning**: Trivy is used for both filesystem and image scans to detect security issues in dependencies and container images.
4. **Vulnerability Detection**: OWASP Dependency Check ensures no known vulnerabilities exist in the application's dependencies.
5. **Deployment**: The application is deployed in Docker containers, orchestrated using Kubernetes on Minikube.

## Security Tools Integration

### SonarQube Analysis
SonarQube is integrated into the Jenkins pipeline to provide continuous inspection of code quality and security. It checks for code bugs, code smells, and security vulnerabilities.

![image](https://github.com/user-attachments/assets/7de7046f-3629-4f30-b4d5-9dce6067f90f)


### Trivy File System & Image Scans
Trivy is used to perform filesystem scans to detect security issues in the project's dependencies. Additionally, image scanning ensures that Docker images are free from vulnerabilities before deployment.

![image](https://github.com/user-attachments/assets/7e8cc6db-157b-45a2-b9da-27cb3901d761)

### OWASP Dependency Check
OWASP Dependency Check is used to analyze the project dependencies for known vulnerabilities. It integrates into the pipeline to halt deployment if critical vulnerabilities are found.

![image](https://github.com/user-attachments/assets/fd64d1d9-1a7d-4ecd-b272-6cba172e63f3)


## Monitoring & Visualization

### Prometheus
Prometheus is configured to monitor the application and the underlying infrastructure. It collects metrics and triggers alerts based on predefined thresholds.

![image](https://github.com/user-attachments/assets/ef2f7f0d-72df-468d-aa4f-f54f8252d608)


### Grafana Dashboards
Grafana is used for visualizing data collected by Prometheus. The dashboards provide insights into the application's performance, resource usage, and any security events detected.

Cluster Monitoring

![image](https://github.com/user-attachments/assets/54a496ae-6daa-4d1a-86cd-e47a9cc66a3a)

Jenkins Monitoring

![image](https://github.com/user-attachments/assets/0872e0a7-130b-43f2-afd4-8dc859153088)


## Application Demo

A fully functional demo of the Netflix Clone application is included, showcasing the final deployment and all integrated security and monitoring tools. (Used Kubectl port forward command to expose service of minikube)

![image](https://github.com/user-attachments/assets/1727b837-c3f6-4f31-81c9-3f03f2892130)


---
### Pipeline Code:
``` 

pipeline {
    agent any
    tools{
            jdk 'java_jdk'
            nodejs 'node16'
        }
        environment {
            SCANNER_HOME=tool 'sonar-scanner'
        }
    stages {
        stage('Clean workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Git Checkout ') {
            steps {
                git branch: 'main', url: 'https://github.com/TanmayRao7/Netflix-Clone.git'
            }
        }
        stage('Sonar Scanner') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh ' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=netflix -Dsonar.projectKey=netflix'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Trivy FS SCAN') {
            steps {
                sh 'trivy fs . > fs-scan.txt'
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build --build-arg TMDB_V3_API_KEY=<API_KEY> -t tanmayrao7/video-apps .'
            }
        }
        stage('Docker Push') {
            steps {
                sh 'docker push tanmayrao7/video-apps:latest'
            }
        }
        stage('Trivy Image Scan') {
            steps {
                sh 'trivy image tanmayrao7/video-apps > image_scan.txt'
            }
        }
        stage('OWASP FS SCAN') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'checker'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage('K8s Deploy') {
            steps {
                sh 'cd Kubernetes && kubectl apply -f deployment.yml'
                sh 'cd Kubernetes && kubectl apply -f service.yml'
                sh 'kubectl get deployments'
            }
        }
    }
}



