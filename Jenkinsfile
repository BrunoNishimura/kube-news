// Declarar o bloco Pipeline:
pipeline {
  agent any

  stages {
//Primeira Parte: Integração Contínua - CI
    stage ('Build Docker Image') {
        steps {
          script {
            dockerapp = docker.build("brunonishimura/kube-news:${env.BUILD_ID}", "-f ./src/Dockerfile ./src")
          }
        }
    }

    stage ('Push Docker Image') {
      steps {
        script {
          docker.withRegistry("https://registry.hub.docker.com", "dockerhub") {
            dockerapp.push("latest")
            dockerapp.push("${env.BUILD_ID}")
          }              
        }
      }
    }
//Segunda Parte: Entrega Contínua - CD
    stage ('Deploy Kubernetes') {
        steps {
          withKubeConfig ([credentialsId: 'kubeconfig']) {
            sh 'kubectl apply -f ./k8s/deployment.yaml'
          }
        }
      }
  }
}