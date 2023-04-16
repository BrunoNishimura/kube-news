// Declarar o bloco Pipeline:
pipeline {
  agent any

  stages {

    stage ('Build Docker Image') {
        steps {
          script {
            dockerapp = docker.build("brunonishimura/kube-news:${env.BUILD_ID}", "-f ./src/Dockerfile ./src")
          }
        }
    }

  }
}