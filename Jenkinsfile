pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    MINIKUBE_CREDENTIALS = credentials('mykubeconfig')
  }
  stages {
    stage('Checkout Source') {
      steps {
        git 'https://github.com/saravana4285/jenkins-kubernetes-deployment.git'
      }
    }
    stage('Build') {
      steps {
        sh 'pwd'
        sh 'ls -lrt'
        sh 'mv Dockerfile.txt Dockerfile'
        sh 'sudo docker build -t saravana4285/sara-app .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push saravana4285/sara-app'
      }
    }
    stage('Deploy in local jenkins server') {
      steps {
        sh 'docker pull saravana4285/sara-app'
        sh 'docker rm -f new-app'
        sh 'docker run -d --name "new-app" saravana4285/sara-app'
      }
    }
    stage('Deploy in Minikube'){
      steps {
        sh 'echo $MINIKUBE_CREDENTIALS | minikube kubectl -- get pods -A'
        sh 'ls -lrt'
        sh 'minikube kubectl -- get pods -A'
    }
      }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
