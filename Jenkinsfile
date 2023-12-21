pipeline {
    agent any

    stages {
        stage("Package") {
            steps {
                sh "./gradlew build"
            }
        }

        stage("Docker build") {
            steps {
                sh "docker build -t calculator ."
            }
        }

        stage("Docker push") {
            steps {
                sh "docker push localhost:5000/calculator"
            }
        }

        stage("Déploiement sur staging") {
            steps {
                sh "docker stop calculator_staging || true"
                sh "docker rm calculator_staging || true"
                sh "docker run -d --rm -p 8765:8080 --name calculator_staging localhost:5000/calculator"
            }
        }

        stage("Test d'acceptation") {
            steps {
                sh "docker stop calculator_acceptance_test || true"
                sh "docker rm calculator_acceptance_test || true"
                sh "docker run -d -p 8765:8080 --name calculator_acceptance_test localhost:5000/calculator"
                sleep 60
                //sh "chmod +x acceptance_test.sh && ./acceptance_test.sh"
            }
        }
    }
    
    post {
        always {
            sh "docker stop calculator_staging || true"
            sh "docker rm calculator_staging || true"
            sh "docker stop calculator_acceptance_test || true"
            sh "docker rm calculator_acceptance_test || true"
        }
    }
}

