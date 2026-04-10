pipeline {
    agent any

    environment {
        IMAGE_NAME = "ciarancarden/fastapi-app"
        DOCKER = "/usr/local/bin/docker"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                sh "${DOCKER} build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Tag Image') {
            steps {
                script {
                    env.TAG = "v1.0.${BUILD_NUMBER}"
                    sh "${DOCKER} tag ${IMAGE_NAME}:latest ${IMAGE_NAME}:${TAG}"
                }
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh """
                    echo \$PASS | ${DOCKER} login -u \$USER --password-stdin
                    ${DOCKER} push ${IMAGE_NAME}:${TAG}
                    """
                }
            }
        }
    }
}
