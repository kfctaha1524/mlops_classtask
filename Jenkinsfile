pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('taha-docker')
  }
  stages {
    stage('Build') {
      steps {
        powershell 'docker build -t model:mymod .'
      }
    }
    stage('Login') {
      steps {
        powershell '$DOCKERHUB_CREDENTIALS_PSW | docker login -u $env:DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push Docker Image') {
      steps {
        withDockerRegistry([credentialsId: "taha-docker", url: "https://index.docker.io/v1/"]) {
          powershell 'docker push tahanoman/mlops:mymod'
        }
      }
    }
    stage('Run Container') {
      steps {
        powershell 'docker run -d -p 5080:80 tahanoman/mlops:mymod'
      }
    }
  }
  post {
    always {
      powershell 'docker logout'
    }
  }
}
