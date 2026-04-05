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

        stage('Build Image') {
            steps {
                sh "${DOCKER} build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Tag Image') {
            steps {
                script {
                    def version = "v1.0.${BUILD_NUMBER}"
                    env.VERSION = version
                    sh "${DOCKER} tag ${IMAGE_NAME}:latest ${IMAGE_NAME}:${version}"
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-creds') {
                        sh "${DOCKER} push ${IMAGE_NAME}:${VERSION}"
                    }
                }
            }
        }
    }
}
