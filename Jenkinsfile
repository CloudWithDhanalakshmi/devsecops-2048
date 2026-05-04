pipeline {
    agent any

    environment {
        AWS_REGION = 'ca-central-1'
        ECR_REPO = '111200748241.dkr.ecr.ca-central-1.amazonaws.com/2048-game'
        IMAGE_TAG = 'latest'
    }

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/CloudwithDhanalakshmi/devsecops-2048.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t 2048-game .'
            }
        }

        stage('Scan with Trivy') {
            steps {
                sh 'trivy image 2048-game'
            }
        }

        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                docker login --username AWS --password-stdin 111200748241.dkr.ecr.ca-central-1.amazonaws.com
                '''
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag 2048-game:latest $ECR_REPO:$IMAGE_TAG'
            }
        }

        stage('Push to ECR') {
            steps {
                sh 'docker push $ECR_REPO:$IMAGE_TAG'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }
    }
}
