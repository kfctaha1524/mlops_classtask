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
        sh 'docker build -t model:mymod .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push Docker Image') {
      steps {
        withDockerRegistry([credentialsId: "taha-docker", url: "https://index.docker.io/v1/"]) {
          sh  "docker push tahanoman/mlops:mymod"
                }
            }
        }
        stage('Run Container') {
      steps {
        sh 'docker run -d -p 5080:80 tahanoman/mlops:mymod'
      }
    }
  }


  post {
    always {
      sh 'docker logout'
    }
  }
}