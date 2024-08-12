pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = 'swapi123'
        DOCKER_IMAGE_NAME = "${DOCKER_HUB_USER}/train-schedule:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/SwapnilM24/cicd-pipeline-train-schedule-autodeploy.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE_NAME} ."
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-password', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                    sh "docker push ${DOCKER_IMAGE_NAME}"
                }
            }
        }
        
        stage('Check kubectl Installation') {
            steps {
                // Check if kubectl is properly install
                sh 'kubectl version --client'
            }
        }
        
        stage('Deploy to EKS') {
            steps {
                // Apply the deployment file
                sh 'kubectl apply -f deployment.yaml --validate=false'
                sh 'cat deployment.yaml'  // Show the contents of deployment.yaml for debugging
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
