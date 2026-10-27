pipeline {
    agent any

    tools {
        maven 'Maven3'
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                bat 'mvn clean test'
            }
        }

        stage('Package Application') {
            steps {
                bat 'mvn package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t campus-equipment-booking:set-a .'
            }
        }

        stage('Deploy to Minikube') {
            steps {
                // Apply Kubernetes Deployment and Service manifests
                bat 'kubectl apply -f deployment.yaml'
                bat 'kubectl apply -f service.yaml'
            }
        }

        stage('Verify Kubernetes Resources') {
            steps {
                // Check deployment, pods, and service status
                bat 'kubectl rollout status deployment/campus-equipment-booking-deployment'
                bat 'kubectl get pods'
                bat 'kubectl get svc'
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}