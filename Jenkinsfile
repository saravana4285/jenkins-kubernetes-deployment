pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
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

    stage('Deploying sara-app container to minikube cluster') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", "service.yaml")
       }
    }
  }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
