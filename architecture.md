

\# MyBookCollection - 3-Tier Architecture



\## 1. Presentation Tier

\- Technology: Angular 13

\- Web Server: Nginx

\- Port: 4200

\- Responsibility: Displays the user interface and communicates with the backend REST APIs.



\## 2. Application Tier

\- Technology: Java 17, Spring Boot

\- Port: 8082 (host), 8080 (container)

\- Responsibility: Handles business logic and REST API requests.



\## 3. Database Tier

\- Technology: MySQL

\- Database: bookDatabase

\- Port: 3306

\- Responsibility: Stores book information and application data.



\## CI/CD Pipeline

1\. Developer pushes source code to GitHub.

2\. Jenkins detects the SCM change.

3\. Jenkins checks required tools.

4\. Jenkins validates Docker Compose configuration.

5\. Jenkins builds Angular and Spring Boot Docker images.

6\. Jenkins deploys the application using Docker Compose.

7\. Jenkins verifies backend API and frontend availability.



\## Supporting Services

\- Docker: Containerization.

\- Docker Compose: Runs application services.

\- Adminer: MySQL database management.

\- GitHub: Source code repository.

\- Jenkins: CI/CD automation.



