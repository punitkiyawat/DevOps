pipeline {
    agent any

    environment {
        IMAGE_NAME = "devops-app"
        CONTAINER_NAME = "devops-container"
        PORT = "5001"
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
                echo "Stopping old container..."
                docker stop $CONTAINER_NAME || true

                echo "Removing old container..."
                docker rm $CONTAINER_NAME || true

                echo "Starting new container on port 5001..."
                docker run -d \
                  --name $CONTAINER_NAME \
                  -p 5001:5000 \
                  --restart unless-stopped \
                  $IMAGE_NAME
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
