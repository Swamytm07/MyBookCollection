
pipeline {
    agent any

    environment {
        GIT = 'C:\\Program Files\\Git\\ucrt64\\bin\\git.exe'
        DOCKER = 'C:\\Program Files\\Docker\\Docker\\resources\\bin\\docker.exe'
        POWERSHELL = 'C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe'
        COMPOSE_PROJECT = 'mybookspipeline'
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Checking out MyBookCollection source code...'

                git branch: 'main',
                    url: 'https://github.com/Swamytm07/MyBookCollection.git'
            }
        }

        stage('Validate Tools') {
            steps {
                echo 'Checking Git, Docker and PowerShell versions...'

                bat '"%GIT%" --version'

                bat '"%DOCKER%" --version'

                bat '"%DOCKER%" compose version'

                bat '"%POWERSHELL%" -NoProfile -Command "$PSVersionTable.PSVersion.ToString()"'
            }
        }

        stage('Validate Docker Compose') {
            steps {
                echo 'Validating Docker Compose configuration...'

                bat '"%DOCKER%" compose -p %COMPOSE_PROJECT% config --quiet'
            }
        }

        stage('Build Docker Images') {
            steps {
                echo 'Building Angular and Spring Boot Docker images...'

                bat '"%DOCKER%" compose -p %COMPOSE_PROJECT% build'
            }
        }

        stage('Deploy Application') {
            steps {
                echo 'Deploying the MyBookCollection application...'

                bat '"%DOCKER%" compose -p %COMPOSE_PROJECT% up -d'
            }
        }

        
stage('Verify Deployment') {
    steps {
        echo 'Checking Docker containers...'

        bat '"%DOCKER%" compose -p %COMPOSE_PROJECT% ps'

        echo 'Checking Spring Boot backend API...'

        bat '''"%POWERSHELL%" -NoProfile -Command "$deadline=(Get-Date).AddSeconds(120); do { try { $r=Invoke-WebRequest -Uri http://localhost:8082/allBooks -UseBasicParsing -TimeoutSec 5; if ($r.StatusCode -eq 200) { Write-Output $r.Content; exit 0 } } catch { Write-Output 'Backend not ready yet'; Start-Sleep -Seconds 5 } } while ((Get-Date) -lt $deadline); Write-Error 'Backend API verification failed'; exit 1"'''

        echo 'Checking Angular frontend...'

        bat '''"%POWERSHELL%" -NoProfile -Command "$r=Invoke-WebRequest -Uri http://localhost:4200/ -UseBasicParsing -TimeoutSec 30; Write-Output $r.StatusCode; if ($r.StatusCode -ne 200) { exit 1 }"'''

        echo 'Checking Adminer...'

        bat '''"%POWERSHELL%" -NoProfile -Command "$r=Invoke-WebRequest -Uri http://localhost:8081/ -UseBasicParsing -TimeoutSec 30; Write-Output $r.StatusCode; if ($r.StatusCode -ne 200) { exit 1 }"'''
    }
}

    }

    post {
        success {
            echo '=============================================='
            echo 'MYBOOKCOLLECTION CI/CD PIPELINE SUCCESS!'
            echo '=============================================='

            echo 'Frontend: http://localhost:4200'
            echo 'Backend API: http://localhost:8082/allBooks'
            echo 'Adminer: http://localhost:8081'

            echo 'All deployment verification stages passed.'
        }

        failure {
            echo '=============================================='
            echo 'MYBOOKCOLLECTION CI/CD PIPELINE FAILED!'
            echo '=============================================='

            echo 'Please check the Jenkins Console Output.'
        }

        always {
            echo 'Pipeline execution completed.'
        }
    }
}
