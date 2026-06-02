pipeline {
    agent any

    environment {
        IMAGE_NAME = "myapp"
        CONTAINER  = "myapp-container"
        APP_PORT   = "8080"
        VM2        = "Service@9.234.41.124"
        REPO       = "https://github.com/Aakarsh-Sinha-Pro/docker-jenkins-pipeline.git"
    }

    stages {
        stage('Deploy on VM2') {
            steps {
                sh '''
                ssh ${VM2} "
                cd /tmp &&
                rm -rf app &&
                git clone ${REPO} app &&
                cd app &&

                docker rm -f ${CONTAINER} || true &&
                docker build -t ${IMAGE_NAME} . &&
                docker run -d -p ${APP_PORT}:80 --name ${CONTAINER} ${IMAGE_NAME}
                "
                '''
            }
        }
    }

    post {
        always {
            sh 'echo Deployment completed!'
        }
    }
}
