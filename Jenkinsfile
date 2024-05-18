pipeline {

  environment {
    dockerimagename = "borispacex/nodeapp"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Verificar GITHUB') {
      steps {
        git 'https://github.com/borispacex/lab-jenkins-udi.git'
      }
    }

    stage('Construir IMAGEN') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }

    stage('Subir IMAGEN') {
      environment {
               registryCredential = 'dockerhublogin'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Implementacion de APP KUBERNETES') {
      steps {
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}