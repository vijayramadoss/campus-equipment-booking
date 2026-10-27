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
                // Load the Docker image into Minikube's local environment
                bat 'minikube image load campus-equipment-booking:set-a'
                
                // Apply Kubernetes manifests with validation disabled to bypass OpenAPI lookup issues
                bat 'kubectl apply --validate=false -f deployment.yaml'
                bat 'kubectl apply --validate=false -f service.yaml'
            }
        }

        stage('Verify Kubernetes Resources') {
            steps {
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