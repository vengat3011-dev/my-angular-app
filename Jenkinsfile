
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Check Node') {
            steps {
                bat 'node -v'
                bat 'npm -v'
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

        stage('Archive Build') {
            steps {
                archiveArtifacts artifacts: 'dist/**/*',
                                 fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Angular Build Successful!'
        }

        failure {
            echo 'Angular Build Failed!'
        }
    }
}
