pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/SwapnilM24/cicd-pipeline-train-schedule-autodeploy.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t swapi123/train-schedule:latest .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-password', variable: 'DOCKER_PASSWORD')]) {
                    sh 'echo $DOCKER_PASSWORD | docker login -u swapi123 --password-stdin'
                    sh 'docker push swapi123/train-schedule:latest'
                }
            }
        }
        stage('Deploy to EKS') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
