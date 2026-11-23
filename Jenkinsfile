
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm ci'
            }
        }

        stage('Angular Build') {
            steps {
                bat 'npx ng build'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t angular-app:latest .'
            }
        }

        stage('Docker Deploy') {
            steps {
                bat '''
                docker rm -f angular-container 2>NUL
                docker run -d --name angular-container -p 8081:80 angular-app:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'Angular Docker Deployment Successful!'
        }

        failure {
            echo 'Build or Deployment Failed!'
        }
    }
}
