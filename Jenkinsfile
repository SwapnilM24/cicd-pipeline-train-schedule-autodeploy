pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = 'swapi123'
        DOCKER_IMAGE_NAME = "${DOCKER_HUB_USER}/train-schedule:latest"
        KUBE_CONFIG_FILE = credentials('kubeconfig-secret') // Use the ID you assigned to the credential
    }

    stages {
        stage('Set Up Kubeconfig') {
            steps {
                script {
                    sh 'mkdir -p ~/.kube'  // Ensure the .kube directory exists
                    sh "cp ${KUBE_CONFIG_FILE} ~/.kube/config"
                }
            }
        }

        stage('Debug Kubeconfig') {
            steps {
                sh 'cat ~/.kube/config'
                sh 'kubectl config get-contexts'  // List all contexts
                sh 'kubectl config current-context'  // Print the current context
            }
        }

        stage('Deploy to EKS') {
            steps {
                sh 'kubectl config use-context arn:aws:eks:us-east-1:339713174488:cluster/EdurekaProject'  // Use the correct context name
                sh 'kubectl apply -f deployment.yaml --validate=false'
            }
        }
    }
}
