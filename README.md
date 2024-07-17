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

