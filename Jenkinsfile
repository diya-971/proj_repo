pipeline {

    agent any

    environment {

        IMAGE_NAME = "apache-demo"

        CONTAINER_NAME = "apache-container"

        REMOTE_USER = "root"

        REMOTE_HOST = "192.168.1.100"

    }

    stages {

        stage('Checkout') {

            steps {

                git branch: 'main',
                url: 'https://github.com/diya-971/proj_repo.git'
            }

        }

        stage('Build Docker Image') {

            steps {

                sh 'docker build -t apache-demo .'

            }

        }

        stage('Deploy to Docker Host') {

            steps {

                sh """

                ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} '

                docker stop ${CONTAINER_NAME} || true

                docker rm ${CONTAINER_NAME} || true

                docker rmi ${IMAGE_NAME} || true

                '

                docker save apache-demo | ssh ${REMOTE_USER}@${REMOTE_HOST} docker load

                ssh ${REMOTE_USER}@${REMOTE_HOST} '

                docker run -d --name ${CONTAINER_NAME} -p 80:80 apache-demo

                '

                """

            }

        }

    }

}
