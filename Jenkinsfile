pipeline {
    agent any

    environment {
        SONARQUBE = 'sonarqube'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/PolinaNechaiko/Deployment_kursova.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv('sonarqube') {
                        sh 'mvn clean install sonar:sonar'
                    }
                }
            }
        }

        stage("Build Docker Image") {
            steps {
                sh 'docker build -t myrepo/artifact:${BUILD_NUMBER} .'
            }
        }

        stage("Push Docker Image") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-credentials-id', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    sh 'docker push myrepo/artifact:${BUILD_NUMBER}'
                }
            }
        }

        stage("Deploy to Nginx") {
            steps {
                sh 'docker run -d --name myapp --network nginx_network -p 80:80 myrepo/artifact:${BUILD_NUMBER}'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed! Womp-Womp'
        }
    }
}

