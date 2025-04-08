pipeline {
    agent any

    environment {
        IMAGE_NAME = 'nodejs-demo-app'
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/DipanshuRawat/nodejs-demo-app.git'
            }
        }

        stage('Build') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials-id', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'docker build -t $USERNAME/$IMAGE_NAME:latest .'
                }
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
