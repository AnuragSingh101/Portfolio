pipeline {
    agent any

    environment {
        DOCKERHUB_USERNAME = 'anus82101'
        DOCKERHUB_PASSWORD = credentials('docker-hub-token') // Store in Jenkins Credentials
        IMAGE_NAME = 'anus82101/portfolio'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/AnuragSingh101/Portfolio.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                sh 'echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin'
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker run -d -p 8080:80 --name portfolio_container $IMAGE_NAME:latest'
            }
        }
    }
    
    post {
        success {
            echo 'Deployment Successful! 🎉'
        }
        failure {
            echo 'Deployment Failed! ❌'
        }
    }
}
