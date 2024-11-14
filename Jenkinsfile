pipeline {
    agent any

    environment {
        SONARQUBE = 'sonarqube'  // the name of your SonarQube server in Jenkins settings
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube Scanner
                    withSonarQubeEnv('sonarqube') {
                        sh 'sonar-scanner'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image (or other build tasks)
            }
        }

        stage('Push Docker Image') {
            steps {
                // Push Docker image (or other tasks)
            }
        }

        stage('Deploy to Nginx') {
            steps {
                // Deploy to Nginx (or other tasks)
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

