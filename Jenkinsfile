pipeline {
  agent any

  environment {
    IMAGE = "vishal1326/portfolio-prod"
    TAG = "${BUILD_NUMBER}"
  }

  stages {

    stage('Build Image') {
      steps {
        sh "docker build -t $IMAGE:$TAG ."
      }
    }

    stage('Push Image') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub-creds',
          usernameVariable: 'DOCKER_USER',
          passwordVariable: 'DOCKER_PASS'
        )]) {

          sh """
          echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
          docker push $IMAGE:$TAG
          docker tag $IMAGE:$TAG $IMAGE:latest
          docker push $IMAGE:latest
          """
        }
      }
    }

    stage('Update Kubernetes Manifest') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'github-creds',
          usernameVariable: 'GIT_USER',
          passwordVariable: 'GIT_PASS'
        )]) {

          sh """
          git checkout main
          git pull origin main --rebase

          sed -i 's|image:.*|image: $IMAGE:$TAG|' k8s/deployment.yaml

          git config user.email "jenkins@local"
          git config user.name "jenkins"

          git add k8s/deployment.yaml
          git commit -m "Update image to $IMAGE:$TAG" || true
          git push https://$GIT_USER:$GIT_PASS@github.com/Vishal5205/devops-portfolio.git main
          """
        }
      }
    }
  }
}
