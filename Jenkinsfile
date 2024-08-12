pipeline {
    agent any

    environment {
        // Docker Hub username
        DOCKER_HUB_USER = 'swapi123'
        // Docker image name
        DOCKER_IMAGE_NAME = "${DOCKER_HUB_USER}/train-schedule:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the GitHub repository
                git 'https://github.com/SwapnilM24/cicd-pipeline-train-schedule-autodeploy.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image
                sh "docker build -t ${DOCKER_IMAGE_NAME} ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                // Push the Docker image to Docker Hub
                withCredentials([string(credentialsId: 'dockerhub-password', variable: 'DOCKER_PASSWORD')]) {
                    sh "echo $DOCKER_PASSWORD | docker login -u ${DOCKER_HUB_USER} --password-stdin"
                    sh "docker push ${DOCKER_IMAGE_NAME}"
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                // Deploy the application to EKS
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
