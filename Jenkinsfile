pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  agent {
    kubernetes {
      yamlFile 'deployment.yaml'
      retries 2
    }
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
    }
  post {
    always {
      sh 'docker logout'
    }
  }
}
