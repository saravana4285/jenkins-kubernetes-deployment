pipeline {

  environment {
    dockerimagename = "saravana4285/flask-app-image"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/saravana4285/CICD-APP-DEMO.git'
        
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build flask-app-image
        }
      }
    }

    stage('Pushing Image') {
      environment {
               registryCredential = 'docker-hub credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
            dockerImage.push("latest")
          }
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yml", "service.yml")
        }
      }
    }

  }

}