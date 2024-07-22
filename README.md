# CI/CD Pipeline of a Demo Spy App

**Java-based Full-Stack Web Application.** Users can view or create missions for each agents. They can also edit/ delete missions.

## Overview of Technologies and Features

- Java: The primary programming language used for application development.
  
- Spring Boot: A framework that simplifies the development of Java applications.

- JDBC: Java Database Connectivity for database interaction.

- H2 Database: An in-memory database used for development and testing.

- Thymeleaf: A server-side Java template engine for rendering HTML.

- HTML5: The standard markup language for creating web pages.

- Maven: A build automation tool used for project management.


## Features

- CRUD Operations: Create, Read, Update, and Delete operations for managing data.

- JDBC for Database Control: Direct interaction with the H2 database.

- Spring Boot Framework: Provides infrastructure support for developing web applications.

- Mapping HTTP Requests: Mapping of HTTP requests to appropriate HTML pages using controllers.

- Sharing Model Attributes: Using Thymeleaf to share model attributes between the controller and HTML files.

- Customizing Schema: Using schema.sql file to customize the database schema.

- Inserting Initial Data: Using data.sql file to insert initial data into the database

## How to Run

1. Clone the repository
2. Open the project in your IDE of choice
3. Run the application

## CI/CD Pipeline Design

Phase 1: Infrastructure Setup

- Create AWS VMs: Provision virtual machines on AWS for Jenkins, Nexus, and SonarQube.

- Install Jenkins: Install Jenkins for continuous integration and continuous deployment.

- Install & Setup SonarQube: Install and configure SonarQube for code quality analysis.

- Install & Setup Nexus: Install and configure Nexus for artifact repository management.

Phase 2: Source Code Setup

Jenkins Plugins Installation & Tool Configuration: Install necessary Jenkins plugins and configure tools required for the CI/CD pipeline.

Phase 3: Declarative CI Pipeline

- CD Stages in Pipeline: Define continuous deployment stages in the pipeline.

- Mail Notifications Configuration: Configure email notifications for pipeline events.

## Implementation Details

Phase 1: Infrastructure Setup

- Create AWS VMs for Jenkins, Nexus, SonarQube
Provision of three virtual machines on AWS to host Jenkins, Nexus, and SonarQube. Each VM will have appropriate specifications to handle the expected load.

![Screenshot 2024-07-17 121609](https://github.com/user-attachments/assets/8dccf1d1-d5bf-4176-a20b-1ae8fa268535)


- Setup AWS EKS with New User
Create a new user with the necessary permissions to manage the EKS cluster. Set up the EKS cluster to deploy and manage Kubernetes applications.

- Install Jenkins
Install Jenkins on the designated VM. Configure Jenkins with the required plugins and integrate it with the version control system (e.g., Git).

![Screenshot 2024-07-17 121826](https://github.com/user-attachments/assets/8ae05524-5513-4877-a2e3-e2dca5722540)


- Install & Setup SonarQube Using Docker Containers
Install SonarQube on the designated VM—Configure SonarQube to integrate with Jenkins for static code analysis. Define quality gates and metrics to ensure code quality.

![Screenshot 2024-07-17 121826](https://github.com/user-attachments/assets/deb2b87c-9a48-4102-acce-6019c52fceb0)

![sonarqube](https://github.com/user-attachments/assets/489708a7-f7f4-4a1a-a935-b73baaf1f061)


- Install & Setup SonatypeNexus3 Using Docker Containers
Install Nexus on the designated VM—Configure Nexus to manage Maven artifacts. Integrate Nexus with Jenkins to automate artifact storage and retrieval.

![sonatype](https://github.com/user-attachments/assets/b8fe01e5-aebc-4b79-b5e8-e415126c2fbd)


Phase 2: Source Code Setup

- Jenkins Plugins Installation & Tool Configuration

- Install necessary Jenkins plugins such as Git, Maven, Kubernetes, and email extension. Configure tools and global settings in Jenkins to prepare for the CI/CD pipelin2

![jenkins](https://github.com/user-attachments/assets/a00a7e83-b237-4e69-82d1-93b6f25ce938)


Phase 3: Declarative CI Pipeline

Define the stages of the continuous deployment pipeline in a Jenkinsfile. The stages include:

- Build: Compile the source code and package it into a deployable artifact.

- Test: Run unit tests, integration tests, and static code analysis.

- Deploy: Deploy the application to a staging environment for further testing.

- Production Deployment: Deploy the application to the production environment upon successful testing

## Jenkins Groovy Pipeline Script

```
pipeline {
    agent any
    
    tools {
        jdk 'Jdk17'
        maven 'Maven3'
    }
    environment{
        SCANNER_HOME= tool "sonar-scanner"
    }

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
        
        stage('Git Checkout') {
            steps {
                git branch: 'main', changelog: false, credentialsId: 'git-cred', poll: false, url: 'https://github.com/Andriouz/Mission-Java-Project.git'
            }
        }
        
        stage('Compile') {
            steps {
                sh "mvn compile"
            }
        }
        
        stage('Test') {
            steps {
                sh "mvn test -DskipTests=true"
            }
        }
        
         stage('Trivy Scan File System') {
            steps {
                sh "trivy fs --format table -o tirvy-fs-report.html ."
                 
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('Sonar') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey=Mission -Dsonar.projectName=Mission \
                            -Dsonar.java.binaries=. '''
                }
                 
            }
        }
        
        stage('Build') {
            steps {
                sh "mvn package -DskipTests=true"
                 
            }
        }
        
        stage('Deploy Artifacts to Nexus') {
            steps {
                withMaven(globalMavenSettingsConfig: 'maven-settings', jdk: 'Jdk17', maven: 'Maven3', mavenSettingsConfig: '', traceability: true) {
                    sh "mvn deploy -DskipTests=true"
                }
                 
            }
        }
        
        stage('Build & Tag Docker Image') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t anderu224/mission:latest ."
                    }
                }
                 
            }
        }
        
         stage('Publish Docker Image') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push anderu224/mission:latest "
                    }
                }
                 
            }
        }
        
    }
}

```

![jenkins1](https://github.com/user-attachments/assets/2ecdf667-7f49-4671-b6c6-5c429843f8d1)

![jenkins 2](https://github.com/user-attachments/assets/29313d7a-2d65-467b-8b53-3f2e74b2e5d0)

**Pushed to Docker Hub**

![docker1](https://github.com/user-attachments/assets/8fb071b0-8282-41be-8192-2c3c5ba42106)


## Conclusion
  
The CI/CD pipeline for the web application is designed to automate the processes of building, testing, and deploying the application.
By leveraging tools such as Jenkins, SonarQube, Nexus, Grafana, and Prometheus, the pipeline ensures continuous integration and continuous delivery while maintaining high code quality and system reliability.
This report provides a detailed implementation plan, which can be followed to set up and maintain the CI/CD pipeline for the given web application.
