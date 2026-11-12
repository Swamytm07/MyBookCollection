
pipeline {
    agent any

    environment {
        DOCKER = 'C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe'
        POWERSHELL = 'C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe'
        GIT = 'C:\\Program Files\\Git\\ucrt64\\bin\\git.exe'
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
                echo 'Checking required tools...'
                bat '"%GIT%" --version'
                bat '"%DOCKER%" --version'
                bat '"%DOCKER%" compose version'
                bat '"%POWERSHELL%" -NoProfile -Command "$PSVersionTable.PSVersion.ToString()"'
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
                echo 'Building Angular and Spring Boot images...'
                bat '"%DOCKER%" compose -p mybookspipeline build'
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Deploying MyBookCollection...'
                bat '"%DOCKER%" compose -p mybookspipeline up -d'
            }
        }

        stage('Verify Deployment') {
            steps {
                echo 'Checking Docker containers...'
                bat '"%DOCKER%" compose -p mybookspipeline ps'

                echo 'Checking Spring Boot backend...'
                bat '"%POWERSHELL%" -NoProfile -Command "$deadline=(Get-Date).AddSeconds(120); do { try { $r=Invoke-WebRequest -Uri http://localhost:8082/allBooks -UseBasicParsing -TimeoutSec 5; if ($r.StatusCode -eq 200) { Write-Output $r.Content; exit 0 } } catch {}; Start-Sleep -Seconds 5 } while ((Get-Date) -lt $deadline); exit 1"'

                echo 'Checking Angular frontend...'
                bat '"%POWERSHELL%" -NoProfile -Command "$r=Invoke-WebRequest -Uri http://localhost:4200/ -UseBasicParsing; if ($r.StatusCode -eq 200) { Write-Output $r.StatusCode; exit 0 } else { exit 1 }"'
            }
        }
    }

    post {
        success {
            echo '======================================'
            echo 'MYBOOKCOLLECTION PIPELINE SUCCESS!'
            echo '======================================'
            echo 'Frontend: http://localhost:4200'
            echo 'Backend:  http://localhost:8082/allBooks'
            echo 'Adminer:  http://localhost:8081'
        }

        failure {
            echo 'MYBOOKCOLLECTION PIPELINE FAILED.'
            echo 'Please check Console Output.'
        }

        always {
            echo 'Pipeline execution completed.'
        }
    }
}
