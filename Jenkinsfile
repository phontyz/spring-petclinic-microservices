pipeline {
    agent any
    stages {
        stage('Shutting down Docker') {
            steps {
                sh "docker-compose down"
            }
        }
        stage('Deleting unused images') {
            steps {
                sh "docker image prune -af"
            }
        }
        stage('Tests') {
            steps {
                sh "./mvnw test"
            }
            
            post {
                always {
                    junit "**/Tests/TEST-*.xml"
                }
            }
        }
        stage('Compiling application') {
            steps {
                sh "./mvnw clean install -P buildDocker"
            }
        }
        stage('Building Docker') {
            steps {
                sh "docker-compose build"
            }
        }
        stage('Composing Docker') {
            steps {       
                sh "docker-compose up -d"
            }
        }
    }
}
