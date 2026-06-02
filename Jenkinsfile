pipeline {
    agent any

    environment {
        DOCKER_HOST = "ssh://Service@9.234.41.124"
        IMAGE = "myapp"
        CONTAINER = "myapp-container"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/progamerzx/docker-pipeline.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t ${IMAGE} .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f ${CONTAINER} || true
                docker run -d -p 8080:80 --name ${CONTAINER} ${IMAGE}
                '''
            }
        }
    }
}
