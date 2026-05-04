pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t 2048-game .'
            }
        }

        stage('Scan with Trivy') {
            steps {
                sh 'trivy image 2048-game || true'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                echo "Working dir:" && pwd
                echo "Files:" && ls -la
                echo "k8s:" && ls -la k8s

                kubectl apply -f k8s/deployment.yaml
                kubectl apply -f k8s/service.yaml
                '''
            }
        }
    }
}
