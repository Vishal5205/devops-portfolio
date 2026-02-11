pipeline {
  agent any

  environment {
    IMAGE = "vishal1326/portfolio-prod"
    TAG = "${BUILD_NUMBER}"
  }

  stages {

    stage('Checkout') {
      steps {
        git credentialsId: 'github-creds',
            url: 'https://github.com/Vishal5205/devops-portfolio'
      }
    }

    stage('Build Image') {
      steps {
        sh "docker build -t $IMAGE:$TAG ."
      }
    }

    stage('Push Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
          usernameVariable: 'DOCKER_USER',
          passwordVariable: 'DOCKER_PASS')]) {

          sh """
          echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
          docker tag $IMAGE:$TAG $IMAGE:latest
          docker push $IMAGE:$TAG
          docker push $IMAGE:latest
          """
        }
      }
    }

    stage('Update Kubernetes Manifest') {
      steps {
        withCredentials([string(credentialsId: 'github-creds', variable: 'GITHUB_TOKEN')]) {
          sh """
          sed -i 's|image:.*|image: $IMAGE:$TAG|' k8s/deployment.yaml
          git config user.email "jenkins@local"
          git config user.name "jenkins"
          git add k8s/deployment.yaml
          git commit -m "Update image to $IMAGE:$TAG"
          git push https://$GITHUB_TOKEN@github.com/Vishal5205/devops-portfolio main
          """
        }
      }
    }
  }
}
