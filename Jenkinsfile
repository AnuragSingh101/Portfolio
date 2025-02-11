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
                powershell "docker build -t $env:IMAGE_NAME:latest ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                powershell "docker login -u $env:DOCKERHUB_USERNAME -p $env:DOCKERHUB_PASSWORD"
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                powershell "docker push $env:IMAGE_NAME:latest"
            }
        }

        stage('Deploy Container') {
            steps {
                powershell "docker run -d -p 8080:80 --name portfolio_container $env:IMAGE_NAME:latest"
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
