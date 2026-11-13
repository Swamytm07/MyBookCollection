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


## Architecture Diagram



```mermaid

flowchart TD

&#x20;   U\[User / Browser]

&#x20;   FE\[Presentation Tier<br/>Angular 13 + Nginx<br/>Port 4200]

&#x20;   BE\[Application Tier<br/>Java 17 + Spring Boot<br/>Port 8082]

&#x20;   DB\[(Database Tier<br/>MySQL<br/>bookDatabase<br/>Port 3306)]



&#x20;   U --> FE

&#x20;   FE -->|REST API| BE

&#x20;   BE -->|Database connection| DB



&#x20;   GH\[GitHub Repository] --> J\[Jenkins CI/CD]

&#x20;   J --> D\[Docker Compose]

&#x20;   D --> FE

&#x20;   D --> BE

&#x20;   D --> DB



&#x20;   A\[Adminer<br/>Port 8081] --> DB

