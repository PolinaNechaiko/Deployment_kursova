pipeline {
    agent any

    environment {
        SONARQUBE = 'sonarqube'  // the name of your SonarQube server in Jenkins settings
        DOCKER_USERNAME = 'pnechaiko'
        DOCKER_PASSWORD = '123Apple123123'
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
                        sonarScanner()
                    }
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Project') {
            steps {
                sh 'npm run build:prod'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t artifact:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Log in to DockerHub
                    sh '''
                    docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
                    docker tag artifact:latest pnechaiko/artifact:latest
                    docker push pnechaiko/artifact:latest
                    '''
                }
            }
        }

        stage('Deploy to Nginx') {
            steps {
                script {
                    // Run docker-compose to deploy Nginx with the container
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

