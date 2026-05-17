pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-app -f docker/Dockerfile .'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker run -d -p 5001:5000 devops-app'
            }
        }
    }
}
