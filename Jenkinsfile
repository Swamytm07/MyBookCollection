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
                    sh 'docker compose build'
                }
            }
        }
        stage('Deploy Application') {
            steps {
                script {
                    sh 'docker compose up -d'
                }
            }
        }
    }
}
