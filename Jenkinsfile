
pipeline {
    agent any

    environment {
        DOCKER = 'C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Swamytm07/MyBookCollection.git'
            }
        }

        stage('Validate Tools') {
            steps {
                echo 'Checking Git and Docker versions...'

                bat '"C:\\Program Files\\Git\\ucrt64\\bin\\git.exe" --version'

                bat '"%DOCKER%" --version'

                bat '"%DOCKER%" compose version'
            }
        }

        stage('Validate Docker Compose') {
            steps {
                echo 'Validating Docker Compose configuration...'

                bat '"%DOCKER%" compose -p mybookspipeline config --quiet'
            }
        }

        stage('Build Docker Images') {
            steps {
                echo 'Building Angular and Spring Boot Docker images...'

                bat '"%DOCKER%" compose -p mybookspipeline build'
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Deploying the MyBookCollection application...'

                bat '"%DOCKER%" compose -p mybookspipeline up -d'
            }
        }

        stage('Verify Deployment') {
            steps {
                echo 'Checking running Docker containers...'

                bat '"%DOCKER%" compose -p mybookspipeline ps'

                echo 'Checking Spring Boot backend API...'

                bat 'powershell -NoProfile -Command "Invoke-RestMethod http://localhost:8082/allBooks -TimeoutSec 120 | ConvertTo-Json -Depth 5"'

                echo 'Checking Angular frontend...'

                bat 'powershell -NoProfile -Command "Invoke-WebRequest http://localhost:4200/ -UseBasicParsing | Select-Object -ExpandProperty StatusCode"'
            }
        }
    }

    post {
        success {
            echo '=========================================='
            echo 'MyBookCollection CI/CD PIPELINE SUCCESS!'
            echo '=========================================='
            echo 'Frontend: http://localhost:4200'
            echo 'Backend:  http://localhost:8082/allBooks'
            echo 'Adminer:  http://localhost:8081'
            echo '=========================================='
        }

        failure {
            echo 'MyBookCollection CI/CD PIPELINE FAILED.'
            echo 'Check the Console Output for the error.'
        }

        always {
            echo 'Pipeline execution completed.'
        }
    }
}
