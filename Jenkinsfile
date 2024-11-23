pipeline {
    agent any
    options{
	timestamps()
}
    stages {
        stage('Getting project') {
            steps {
                // Клонування репозиторію
                sh "rm -rf Deployment_kursova; git clone https://github.com/PolinaNechaiko/Deployment_kursova.git"
            }
        }

        stage('Getting NPM') {
            steps {
                // Встановлення npm
                sh "sudo apt install -y npm"
            }
        }

        stage('Installing dependencies') {
            steps {
                // Встановлення залежностей
                sh "npm install --prefix ./Deployment_kursova"
            }
        }

        stage('Building artifact') {
            steps {
                // Збірка артефакту
                sh "npm run build:prod --prefix ./Deployment_kursova"
            }
        }

        stage('SonarQube analysis') {
            steps {
                script {
                    // Налаштування SonarQube та запуск аналізу
                    def scannerHome = tool 'Scanner'
                    withSonarQubeEnv('sonarqube') {
                        // Запуск SonarQube Scanner
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectBaseDir=./Delpoyment_kursova"
                    }
                }
            }
        }

        stage('Quality Gate check') {
            steps {
                script {
                    // Wait for the quality gate to pass/fail
                    def qualityGate = waitForQualityGate('sonarqube')  // 'sonarqube' is the name of the SonarQube server in Jenkins
                    if (qualityGate.status != 'OK') {
                        error "Quality Gate failed: ${qualityGate.status}"
                    }
                }
            }
        }

 	stage('Building Docker image with Nginx') {
            steps {
                script {
                    // Створення Docker образу Nginx із побудованим артефактом
                    sh """
                        docker build -t my-nginx-image -f ./Deployment_kursova/Dockerfile .
                    """
                }
            }
        }

	stage('Deploying Docker container') {
            steps {
                script {
		    def containerExists = sh(script: "docker ps -a -q -f name=my-nginx-container", returnStdout: true).trim()

		    if (containerExists){
		    sh """
			docker stop my-nginx-container
			docker rm my-nginx-container
			"""
		    }
                    // Запуск контейнера з новим образом Nginx
                    sh """
                        docker run -d -p 80:80 --name my-nginx-container my-nginx-image
                    """
                }
            }
        }
    }
}

