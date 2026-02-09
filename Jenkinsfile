pipeline {
    agent any

    environment {
        IMAGE_NAME = "vishal1326/portfolio:latest"
        GIT_REPO   = "https://github.com/Vishal5205/devops-portfolio.git"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git "${GIT_REPO}"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t ${IMAGE_NAME} .
                """
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                sh """
                docker login
                docker push ${IMAGE_NAME}
                """
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh """
                kubectl apply -f k8s/
                """
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully"
        }
        failure {
            echo "Pipeline failed. Check console logs."
        }
    }
}
