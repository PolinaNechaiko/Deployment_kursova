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
                        sonarqubeSscanner()
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t artifact:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
		docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
		docker tag artifact:latest pnechaiko/artifact:latest
		docker push pnechaiko/artifact:latest
		'''
            }
        }

        stage('Deploy to Nginx') {
            steps {
                sh 'docker-compose up -d'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

