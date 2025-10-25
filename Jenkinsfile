pipeline {
    agent any

    environment {
        DOCKERHUB_USER = "akshayps17"                      // your Docker Hub username
        IMAGE_NAME = "node-login-module"                   // image name only
        IMAGE_TAG = "latest"                               // tag version
        DOCKER_CREDENTIALS = "dockerhub-credentials"       // Jenkins credential ID
    }

    stages {

        stage('Checkout') {
            steps {
                echo "📦 Cloning repository..."
                git branch: 'test', url: 'https://github.com/akshayps17/nodejs-demo-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "🐳 Building Docker image..."
                script {
                    docker.build("${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo "📤 Pushing Docker image to Docker Hub..."
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS}") {
                        docker.image("${DOCKERHUB_USER}/${IMAGE_NAME}:${IMAGE_TAG}").push()
                    }
                }
            }
        }

        stage('Clean Up') {
            steps {
                echo "🧹 Cleaning up unused Docker images..."
                // Use `bat` for Windows Jenkins or `sh` for Linux
                bat 'docker image prune -f'
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully and image pushed to Docker Hub!"
        }
        failure {
            echo "❌ Pipeline failed. Please check logs for details."
        }
    }
}
