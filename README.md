# Spy App

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

- RBAC Setup in EKS Cluster: Set up Role-Based Access Control (RBAC) in the EKS cluster.

- CD Stages in Pipeline: Define continuous deployment stages in the pipeline.

- Mail Notifications Configuration: Configure email notifications for pipeline events.

## Implementation Details

Phase 1: Infrastructure Setup

- Create AWS VMs for Jenkins, Nexus, SonarQube
Provision of three virtual machines on AWS to host Jenkins, Nexus, and SonarQube. Each VM will have appropriate specifications to handle the expected load.

- Setup AWS EKS with New User
Create a new user with the necessary permissions to manage the EKS cluster. Set up the EKS cluster to deploy and manage Kubernetes applications.

- Install Jenkins
Install Jenkins on the designated VM. Configure Jenkins with the required plugins and integrate it with the version control system (e.g., Git).

- Install & Setup SonarQube Using Docker Containers
Install SonarQube on the designated VM—Configure SonarQube to integrate with Jenkins for static code analysis. Define quality gates and metrics to ensure code quality.

- Install & Setup Nexus Using Docker Containers
Install Nexus on the designated VM—Configure Nexus to manage Maven artifacts. Integrate Nexus with Jenkins to automate artifact storage and retrieval.

Phase 2: Source Code Setup

- Jenkins Plugins Installation & Tool Configuration

- Install necessary Jenkins plugins such as Git, Maven, Kubernetes, and email extension. Configure tools and global settings in Jenkins to prepare for the CI/CD pipeline.

Phase 3: Declarative CI Pipeline

Define the stages of the continuous deployment pipeline in a Jenkinsfile. The stages include:

- Build: Compile the source code and package it into a deployable artifact.

- Test: Run unit tests, integration tests, and static code analysis.

- Deploy: Deploy the application to a staging environment for further testing.

- Production Deployment: Deploy the application to the production environment upon successful testing

## Conclusion
  
The CI/CD pipeline for the web application is designed to automate the processes of building, testing, and deploying the application.
By leveraging tools such as Jenkins, SonarQube, Nexus, Grafana, and Prometheus, the pipeline ensures continuous integration and continuous delivery while maintaining high code quality and system reliability.
This report provides a detailed implementation plan, which can be followed to set up and maintain the CI/CD pipeline for the given web application.
