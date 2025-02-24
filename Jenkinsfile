pipeline {
  agent any

  environment {
    GCP_PROJECT = 'phrasal-verve-447910-d9'
    IMAGE_NAME = 'hello-world-app'
    IMAGE_TAG = 'latest'
    GCR_PATH = "gcr.io/${GCP_PROJECT}/${IMAGE_NAME}:${IMAGE_TAG}"
  }

  stages {
    stage('Clone Repo') {
      steps {
        git 'https://github.com/MeghanaMeghas/Hello-World.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
        }
      }
    }

    stage('Push to GCR') {
      steps {
        script {
          sh """
            docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${GCR_PATH}
            docker push ${GCR_PATH}
          """
        }
      }
    }

    stage('Deploy to GKE') {
      steps {
        script {
          sh """
            gcloud container clusters get-credentials jenkins-cd --zone us-central1-a --project ${GCP_PROJECT}
            kubectl apply -f k8s/deployment.yaml
          """
        }
      }
    }
  }
}
