# jenkins-demo
This is a jenkins CICD demo project.

# Static HTML Site with Docker + Jenkins CI/CD

## Overview
This repository contains a static HTML website that is hosted using Docker + NGINX.  
A Jenkins CI/CD pipeline is configured to automate the build and deployment process.

---

## Hosting Locally with Docker

### 1. Build Docker Image
```bash
docker build -t my-static-site .
```

### 2. Run the Container
```bash
docker run -d -p 8080:80 my-static-site
```

### 3. Access the Site
Open your browser:
```
http://localhost:8080
```

---

## Quick Test Without Dockerfile
If you just want to test the site locally without building a custom image:
```bash
docker run -d -p 8080:80 -v $(pwd):/usr/share/nginx/html:ro nginx:alpine
```

---

## Jenkins CI/CD Pipeline

### Pipeline Stages
1. Checkout Code – Pulls the latest code from GitHub.
2. Build Docker Image – Builds a Docker image from the static site files.
3. Push to Registry (Optional) – Pushes the image to Docker Hub or a private registry.
4. Deploy – Runs the updated container with the new image.

### Example Jenkinsfile
```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/UnstopableSafar08/jenkins-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-static-site .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f static-site || true
                docker run -d --name static-site -p 8080:80 my-static-site
                '''
            }
        }
    }
}
```

---

## Requirements
- [Docker](https://docs.docker.com/get-docker/)
- [Jenkins](https://www.jenkins.io/download/)
- GitHub Repository (this repo)

---

## Access
Once the pipeline runs successfully, visit:
```
http://<server-ip>:8080
```

---

## Author
- Sagar Malla  
- Email: sagarmalla08
