pipeline {
    agent any
    environment {
        IMAGE_NAME = 'varunmiv/task-2'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Varunmiv/task-2.git'
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test || echo "No tests specified"'
            }
        }
        stage('Docker Build & Push') {
            steps {
                script {
                    docker.build(task-2).push('latest')
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker run -d -p 3000:3000 --name app ' + IMAGE_NAME
            }
        }
    }
}
