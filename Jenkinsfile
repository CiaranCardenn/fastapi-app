pipeline {
    agent any

    environment {
        IMAGE_NAME = "ciarancarden/fastapi-app"
        DOCKER = "/usr/local/bin/docker"
    }

    stages {

        stage('Get Code') {
            steps {
                checkout scm
            }
        }

        stage('Build & Push Image') {
            steps {
                sh """
                ${DOCKER} buildx create --use || true

                ${DOCKER} buildx build \
                --platform linux/amd64 \
                -t ${IMAGE_NAME}:v1.0.${BUILD_NUMBER} \
                -t ${IMAGE_NAME}:latest \
                --push .
                """
            }
        }
    }
}
