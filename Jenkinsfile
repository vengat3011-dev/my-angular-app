
pipeline {
    agent any

    stages {

        // 1. Checkout Source Code
        stage('Git Checkout') {
            steps {
                echo 'Checking out Angular project'
                checkout scm
            }
        }

        // 2. Check Node and NPM
        stage('Check Environment') {
            steps {
                bat 'node -v'
                bat 'npm -v'
                bat 'docker --version'
            }
        }

        // 3. Install Dependencies
        stage('Install Dependencies') {
            steps {
                echo 'Installing npm dependencies'
                bat 'npm ci'
            }
        }

        // 4. Angular Production Build
        stage('Angular Build') {
            steps {
                echo 'Building Angular application'
                bat 'npx ng build'
            }
        }

        // 5. Docker Image Build
        stage('Docker Build') {
            steps {
                echo 'Creating Docker image'
                bat 'docker build -t angular-app:latest .'
            }
        }

        // 6. Remove Old Container
        stage('Remove Old Container') {
            steps {
                echo 'Removing old Angular container'
                bat 'docker rm -f angular-container 2>NUL || exit /b 0'
            }
        }

        // 7. Deploy New Container
        stage('Deploy New Container') {
            steps {
                echo 'Starting new Angular container'
                bat 'docker run -d --restart unless-stopped --name angular-container -p 8081:80 angular-app:latest'
            }
        }

        // 8. Verify Container
        stage('Verify Deployment') {
            steps {
                bat 'docker ps'
                echo 'Angular application deployed successfully'
            }
        }
    }

    post {
        success {
            echo '================================'
            echo 'ANGULAR CI/CD SUCCESS'
            echo 'Application: http://localhost:8081'
            echo '================================'
        }

        failure {
            echo 'Angular CI/CD Pipeline Failed!'
        }
    }
}
