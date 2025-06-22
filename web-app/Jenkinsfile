pipeline {
  agent any

  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials') // username & password
  }

  stages {
    stage('Clone') {
      steps {
        git 'https://github.com/your-org/web-app.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        script {
          docker.build("web-app:latest", "./backend")
        }
      }
    }

    stage('Push to DockerHub') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
            docker.image("web-app:latest").push()
          }
        }
      }
    }

    stage('Deploy to K8s') {
      steps {
        sh '''
          kubectl apply -f k8s/deployment.yaml
          kubectl apply -f k8s/service.yaml
          kubectl apply -f k8s/ingress.yaml
        '''
      }
    }
  }
}

