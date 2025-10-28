pipeline {
    agent any

    tools {
        nodejs "NodeJS_18"
    }

    environment {
        APP_NAME = "node-app"
        DOCKER_IMAGE = "${APP_NAME}:latest"
        CONTAINER_PORT = "3000"
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📦 Cloning repository...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo '📦 Installing Node.js dependencies...'
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker image...'
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Run Container') {
            steps {
                echo '🚀 Running Docker container locally...'
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
            echo "✅ Deployment successful! App is running at http://<your-server-ip>:${CONTAINER_PORT}"
        }
        always {
            echo '🧹 Cleaning workspace...'
            cleanWs()
        }
    }
}
