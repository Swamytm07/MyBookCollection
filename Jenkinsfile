pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Swamytm07/MyBookCollection.git'
            }
        }
        stage('Build Docker Images') {
            steps {
                script {
                    bat 'docker compose build'
                }
            }
        }
        stage('Deploy Application') {
            steps {
                script {
                    bat 'docker compose up -d'
                }
            }
        }
    }
}
