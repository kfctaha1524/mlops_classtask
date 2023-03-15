pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
     DOCKERHUB_USERNAME = 'tahanoman'
    DOCKERHUB_PASSWORD = 'dckr_pat_uLfY0D6GnLOE6IrV0vI3WA5uzEk'
  }
  stages {
    stage('Build') {
      steps {
        powershell 'docker build -t model:mymod .'
      }
    }
    stage('Login') {
      steps {
        powershell 'echo %DOCKERHUB_PASSWORD% | docker login -u %DOCKERHUB_USERNAME% --password-stdin'

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
