pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ciarancarden/fastapi-app"
        DOCKER_TAG = "v1.0.${BUILD_NUMBER}"
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
                    # Enable buildx (only needed once, safe to run every time)
                    docker buildx create --use || true
                    docker buildx inspect --bootstrap

                    # Build for AMD64 and push
                    docker buildx build \
                        --platform linux/amd64 \
                        -t $DOCKER_IMAGE:$DOCKER_TAG \
                        -t $DOCKER_IMAGE:latest \
                        --push .
                    '''
                }
            }
        }

        stage('Print Image Info') {
            steps {
                echo "Docker image pushed: ${DOCKER_IMAGE}:${DOCKER_TAG}"
            }
        }
    }
}
