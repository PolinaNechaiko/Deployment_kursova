pipeline {
    agents: any

    environment {
        SONARQUBE = 'sonarqube'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/PolinaNechaiko/Deployment_kursova.git'
            }
        }

        stage ('SonarQube Analysis') {
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
                sh 'docker build -t artifact .'
            }
        }

        stage("Push Docker Image") {
            steps {
                sh 'docker push artifact'
            }
        }

        stage("Deploy to Nginx") {
            steps {
                sh 'docker run -d -p 80:80 artifact'
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