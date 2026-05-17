pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/punitkiyawat/devops-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-app -f docker/Dockerfile .'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker run -d -p 5000:5000 devops-app'
            }
        }
    }
}
