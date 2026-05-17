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

        stage('Free Port') {
            steps {
                sh '''
                echo "Freeing port 5001 if used..."

                # Kill any process using port 5001
                sudo fuser -k 5001/tcp || true

                echo "Stopping all running containers (safe)..."
                docker ps -q | xargs -r docker stop || true

                echo "Removing all containers..."
                docker ps -aq | xargs -r docker rm || true
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                echo "Starting new container..."

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
