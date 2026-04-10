pipeline {
    agent any

    environment {
        IMAGE = "ciarancarden/fastapi-app"
        TAG = "v1.0.${BUILD_NUMBER}"
        PATH = "/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
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
                    echo "Using Docker at:"
                    which docker

                    docker --version

                    # Setup buildx (safe)
                    docker buildx create --use || true
                    docker buildx inspect --bootstrap

                    # Build + push AMD64 image
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
