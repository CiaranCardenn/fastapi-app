pipeline {
    agent any

    environment {
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
                    # Verify docker exists
                    which docker
                    docker --version

                    # Setup buildx (safe)
                    docker buildx create --use || true
                    docker buildx inspect --bootstrap

                    # Build AMD64 image and push
                    docker buildx build \
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
