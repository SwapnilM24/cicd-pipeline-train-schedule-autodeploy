pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = 'swapi123'
        DOCKER_IMAGE_NAME = "${DOCKER_HUB_USER}/train-schedule:latest"
        KUBE_CONFIG_FILE = credentials('kubeconfig-secret') // Replace with your actual credential ID
    }

    stages {
        stage('Set Up Kubeconfig') {
            steps {
                script {
                    // Copy the kubeconfig file to the correct location
                    sh 'mkdir -p ~/.kube'  // Ensure the .kube directory exists
                    sh "cp ${KUBE_CONFIG_FILE} ~/.kube/config"
                }
            }
        }

        // Other stages (Checkout, Build, Push, Deploy) follow here...

        stage('Deploy to EKS') {
            steps {
                sh 'kubectl config current-context'  // Check current context
                sh 'kubectl apply -f deployment.yaml --validate=false'
            }
        }
    }
}
