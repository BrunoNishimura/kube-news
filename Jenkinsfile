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
        environments{
          tag_version = "${env.BUILD_ID}"
        }
        steps {
          withKubeConfig ([credentialsId: 'kubeconfig']) {
            sh 'sed -i "s/{{TAG}}/$tag_version/g" ./k8s/deployment.yaml'
            sh 'kubectl apply -f ./k8s/deployment.yaml'
          }
        }
      }
  }
}