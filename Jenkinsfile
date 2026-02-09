pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "vishal1326/portfolio"
        GIT_REPO = "https://github.com/Vishal5205/devops-portfolio.git"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: "${GIT_REPO}",
                    credentialsId: 'github-creds'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:latest ."
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh """
                    echo \$PASS | docker login -u \$USER --password-stdin
                    docker push ${DOCKER_IMAGE}:latest
                    """
                }
            }
        }

        stage('Update Kubernetes YAML') {
            steps {
                sh """
                sed -i 's|image:.*|image: ${DOCKER_IMAGE}:latest|g' k8s/deployment.yaml
                """
            }
        }

        stage('Push YAML to GitHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github-creds', usernameVariable: 'GUSER', passwordVariable: 'GPASS')]) {
                    sh """
                    git config user.email "jenkins@local"
                    git config user.name "jenkins"

                    git add k8s/deployment.yaml
                    git commit -m "Update image via Jenkins" || echo "No changes"
                    git push https://\$GUSER:\$GPASS@github.com/Vishal5205/devops-portfolio.git main
                    """
                }
            }
        }
    }
}
