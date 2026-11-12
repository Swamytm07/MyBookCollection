
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
                bat 'git --version'
                bat '"%DOCKER%" --version'
                bat '"%DOCKER%" compose version'
            }
        }

        stage('Validate Docker Compose') {
            steps {
                bat '"%DOCKER%" compose -p mybookspipeline config --quiet'
            }
        }

        stage('Build Docker Images') {
            steps {
                bat '"%DOCKER%" compose -p mybookspipeline build'
            }
        }

        stage('Deploy Application') {
            steps {
                bat '"%DOCKER%" compose -p mybookspipeline up -d'
            }
        }

        stage('Verify Deployment') {
            steps {
                bat '"%DOCKER%" compose -p mybookspipeline ps'
                bat 'curl.exe -f http://localhost:8082/allBooks'
                bat 'curl.exe -f http://localhost:4200'
            }
        }
    }
}
