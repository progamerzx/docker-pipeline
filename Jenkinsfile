pipeline {
    agent any

    environment {
        IMAGE_NAME = "myapp"
        IMAGE_TAG  = "${BUILD_NUMBER}"        // unique tag per build
        CONTAINER  = "myapp-container"
        APP_PORT   = "8080"
        // If you prefer an env var over a docker context, uncomment:
        // DOCKER_HOST = "ssh://ec2-user@<SERVER_B_PRIVATE_IP>"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm                  // pulls this repo into the workspace
            }
        }

        stage('Verify remote Docker') {
            steps {
                sh 'docker version'           // confirms client + remote daemon
                sh 'docker info | head -20'
            }
        }

        stage('Build image') {
            steps {
                // build context (the workspace) is streamed over SSH to Server B
                sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
            }
        }

        stage('Run container') {
            steps {
                sh '''
                    docker rm -f ${CONTAINER} || true
                    docker run -d --name ${CONTAINER} \
                        -p ${APP_PORT}:8080 \
                        ${IMAGE_NAME}:${IMAGE_TAG}
                '''
            }
        }

        stage('Smoke test') {
            steps {
                sh 'sleep 3'
                sh 'docker ps --filter name=${CONTAINER}'
            }
        }
    }

    post {
        always {
            sh 'docker ps'                    // runs against Server B
        }
    }
}