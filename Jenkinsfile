
pipeline {
    agent any

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
                bat 'docker --version'
                bat 'docker compose version'
            }
        }

        stage('Validate Docker Compose') {
            steps {
                bat 'docker compose -p mybookspipeline config --quiet'
            }
        }

        stage('Build Docker Images') {
            steps {
                bat 'docker compose -p mybookspipeline build'
            }
        }

        stage('Deploy Application') {
            steps {
                bat 'docker compose -p mybookspipeline up -d'
            }
        }

        stage('Verify Deployment') {
            steps {
                bat 'docker compose -p mybookspipeline ps'
                bat 'curl.exe -f http://localhost:8082/allBooks'
                bat 'curl.exe -f http://localhost:4200'
            }
        }
    }
}
