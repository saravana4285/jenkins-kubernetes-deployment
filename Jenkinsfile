pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }

  stages {
    stage('Checkout Source') {
      steps {
        git 'https://github.com/Bravinsimiyu/jenkins-kubernetes-deployment.git'
      }
    }
    stage('Build') {
      steps {
        sh 'pwd'
        sh 'ls -lrt'
        sh 'mv Dockerfile.txt Dockerfile'
        sh 'sudo docker build -t $dockerimagename .'
      }
    }
    stage('Pushing Image') {
      environment {
        registryCredential = 'dockerhub-credentials'
      }
      steps{
        script {
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) {
          sh 'docker push docker.io/saravana4285/sara-app'
          }
        }
      }
    }
   }
  post {
    always {
      sh 'sudo docker logout'
    }
  }
}
