
pipeline {
    agent any

    options {
        timestamps()
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code checked out from GitHub.'
            }
        }

        stage('Check Node.js and npm') {
            steps {
                sh 'node --version'
                sh 'npm --version'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Validate JavaScript') {
            steps {
                sh 'npm test'
            }
        }
    }

    post {
        success {
            echo 'Express CI pipeline completed successfully.'
        }
        failure {
            echo 'Express CI pipeline failed. Review the console output.'
        }
    }
}
