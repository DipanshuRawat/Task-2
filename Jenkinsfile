pipeline {
    agent any

    environment {
        IMAGE_NAME = 'nodejs-demo-app'
        DOCKER_HUB_USER = credentials('dockerhub-credentials-id')
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/DipanshuRawat/nodejs-demo-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t $DOCKER_HUB_USER/$IMAGE_NAME:latest .'
            }
        }

        stage('Test') {
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }

        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials-id', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                    sh 'docker push $USERNAME/$IMAGE_NAME:latest'
                }
            }
        }
    }

    post {
        failure {
            echo '❌ Build failed!'
        }
        success {
            echo '✅ Build and push successful!'
        }
    }
}
