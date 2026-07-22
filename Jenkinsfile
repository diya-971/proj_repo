pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-website"
        CONTAINER_NAME = "my-website-container"
    }

    stages {

        stage('Checkout Source') {
            steps {
                // Jenkins automatically checks out the repository
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:latest .'
            }
        }

        stage('Stop Existing Container') {
            steps {
                sh '''
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker run -d \
                --name ${CONTAINER_NAME} \
                -p 8080:80 \
                ${IMAGE_NAME}:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed.'
        }

        always {
            sh 'docker images'
            sh 'docker ps -a'
        }
    }
}
