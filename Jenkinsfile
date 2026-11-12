 
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
                bat '"C:\\Program Files\\Git\\ucrt64\\bin\\git.exe" --version'
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

                bat 'powershell -NoProfile -Command "$deadline=(Get-Date).AddSeconds(120); do { try { $r=Invoke-WebRequest -Uri http://localhost:8082/allBooks -UseBasicParsing -TimeoutSec 5; if ($r.StatusCode -eq 200) { Write-Output $r.Content; exit 0 } } catch {}; Start-Sleep -Seconds 5 } while ((Get-Date) -lt $deadline); exit 1"'

                bat 'powershell -NoProfile -Command "$r=Invoke-WebRequest -Uri http://localhost:4200/ -UseBasicParsing; if ($r.StatusCode -ne 200) { exit 1 }; Write-Output (''Frontend HTTP status: '' + $r.StatusCode)"'
            }
        }
    }
}
