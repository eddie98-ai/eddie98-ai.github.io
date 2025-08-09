---
layout: page
title: On-Prem Hosting & CI/CD Infrastructure
description: Architected and deployed a self-hosted GitLab and Jenkins platform for robust, automated software delivery.
img: assets/img/ci_cd_architecture.png
importance: 5
category: DevOps & MLOps Infrastructure
---

While cloud-based CI/CD platforms are common, building an on-premises infrastructure provides complete control over security, data privacy, and the development toolchain. This project involved architecting and deploying a robust, self-hosted CI/CD platform from the ground up on a dedicated Linux server.

The goal was to create a reliable and automated system for building, testing, and deploying multi-repository backend and frontend applications, managed entirely in-house.

---

### 1. System Architecture

The platform is built around two core components: a **GitLab** instance for source code management and a **Jenkins** server for automation. The entire system is hosted on a Linux (Ubuntu) server configured with a static IP and DNS for stable, reliable access.

---

### 2. Key Components

*   **Self-Hosted GitLab Instance:** Serves as the central hub for all source code. Its key responsibilities include:
    *   **Source Code Management (SCM):** Hosting Git repositories for various backend and frontend services.
    *   **Access Control:** Managing user and group permissions with fine-grained controls to protect critical branches like `main`.
    *   **Webhook Triggers:** Automatically notifying the Jenkins server via webhooks whenever new code is pushed to a repository.

*   **Jenkins Automation Server:** Acts as the workhorse of the CI/CD pipeline. Its roles include:
    *   Listening for webhooks from GitLab to start new jobs.
    *   Executing scripted pipelines (`Jenkinsfile`) to define build, test, and deployment stages.
    *   Managing build artifacts and reporting the status of each pipeline run.

---

### 3. The Automated CI/CD Pipeline

A typical pipeline for a containerized application was configured with the following stages:

<div style="background-color: #f8f9fa; border: 1px solid #e9ecef; padding: 20px; border-radius: 5px; margin-top: 1.5rem; margin-bottom: 2rem;">
  <h5 style="margin-top:0; border-bottom: 2px solid #dee2e6; padding-bottom: 10px; margin-bottom: 15px;">Pipeline Stages Pseudocode</h5>
{% highlight groovy %}
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                // 1. Compile code, install dependencies
                sh 'npm install'
                sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                // 2. Run automated tests (unit, integration)
                sh 'npm test'
            }
        }
        stage('Package') {
            steps {
                // 3. Build a Docker image for the application
                sh 'docker build -t my-app:${BUILD_NUMBER} .'
            }
        }
        stage('Deploy') {
            steps {
                // 4. Push to a registry and deploy to a server
                sh 'docker push my-registry/my-app:${BUILD_NUMBER}'
                sh 'ssh user@deploy-server "docker pull ... && docker run ..."'
            }
        }
    }
}
{% endhighlight %}
</div>

<br>

---
### Technical Summary
- **Source Control:** Self-hosted GitLab
- **Automation Server:** Jenkins
- **Containerization:** Docker
- **Operating System:** Ubuntu Linux
- **Networking:** Static IP configuration, DNS (Bind9)