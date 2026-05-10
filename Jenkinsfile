pipeline {
    agent any

    environment {
        IMAGE_NAME = "sync-service"
        CONTAINER_NAME = "sync-service-container"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Application') {
            steps {
                sh './gradlew clean build'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f $CONTAINER_NAME || true
                    docker run -d -p 8081:8080 --name $CONTAINER_NAME $IMAGE_NAME
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh 'sleep 10'
                sh 'curl http://localhost:8081/actuator/health'
            }
        }
    }
}
