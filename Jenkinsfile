pipeline {
    agent any

    environment {
        DOCKER = "/opt/homebrew/bin/docker"
        IMAGE = "ciarancarden/fastapi-app"
        TAG = "v1.0.${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build & Push Docker Image') {
            steps {
                script {
                    sh '''
                    # Setup buildx (safe to run every time)
                    $DOCKER buildx create --use || true
                    $DOCKER buildx inspect --bootstrap

                    # Build AMD64 image and push
                    $DOCKER buildx build \
                        --platform linux/amd64 \
                        -t $IMAGE:$TAG \
                        -t $IMAGE:latest \
                        --push .
                    '''
                }
            }
        }

        stage('Print Image Info') {
            steps {
                echo "Docker image pushed: ${IMAGE}:${TAG}"
            }
        }
    }
}
