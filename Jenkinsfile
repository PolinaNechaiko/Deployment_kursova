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
                        sh "cd Deployment_kursova && ${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
    }
}

