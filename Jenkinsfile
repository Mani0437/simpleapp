pipeline {
    agent any

    environment {
        APP_NAME = "node-app"
        DOCKER_IMAGE = "${APP_NAME}:latest"
        CONTAINER_PORT = "8083"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Run Container') {
            steps {
                echo 'Running container locally...'
                sh """
                docker stop ${APP_NAME} || true
                docker rm ${APP_NAME} || true
                docker run -d -p ${CONTAINER_PORT}:${CONTAINER_PORT} --name ${APP_NAME} ${DOCKER_IMAGE}
                """
            }
        }
    }

    post {
        success {
            echo "✅ Application is running locally at http://<your-server-ip>:${CONTAINER_PORT}"
        }
        always {
            echo 'Cleaning up workspace...'
            cleanWs()
        }
    }
}
