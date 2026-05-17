pipeline {
    agent any

    environment {
        IMAGE_NAME = "devops-app"
        CONTAINER_NAME = "devops-container"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                echo "Building Docker image..."
                docker build -t $IMAGE_NAME -f docker/Dockerfile .
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                echo "Stopping existing container (if any)..."
                docker stop $CONTAINER_NAME || true

                echo "Removing existing container..."
                docker rm $CONTAINER_NAME || true

                echo "Starting container on a FREE random port..."

                docker run -d \
                  --name $CONTAINER_NAME \
                  -P \
                  --restart unless-stopped \
                  $IMAGE_NAME

                echo "Container started successfully ✅"

                echo "Check running container:"
                docker ps
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Deployment failed!'
        }
    }
}
