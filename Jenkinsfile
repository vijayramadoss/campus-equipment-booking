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
                // Builds the Docker image with the required tag for Task 5
                bat 'docker build -t campus-equipment-booking:set-a .'
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}