pipeline {
    agent any

    environment {
        KUBECTL = "microk8s kubectl"
        APP_NAME = "dvwa"
        NAMESPACE = "default"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Cluster connectivity') {
            steps {
                sh """
                  $KUBECTL get nodes
                  $KUBECTL get pods -A
                """
            }
        }

        stage('Deploy DVWA') {
            steps {
                sh """
                  $KUBECTL apply -f k8s/dvwa-deployment.yaml
                  $KUBECTL apply -f k8s/dvwa-service.yaml
                """
            }
        }

        stage('Verify Deployment') {
            steps {
                sh """
                  $KUBECTL rollout status deployment/dvwa
                  $KUBECTL get svc dvwa
                """
            }
        }
    }

    post {
        success {
            echo "DVWA deployed successfully on MicroK8s"
        }
        failure {
            echo "Deployment failed"
        }
    }
}