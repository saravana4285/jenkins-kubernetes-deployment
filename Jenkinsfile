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
        sh 'sudo su - cicd-vm1 -c "/home/cicd-vm1/.minikube/cache/linux/amd64/v1.26.3/kubectl config set-cluster minikube --server=https://192.168.0.105:8443 --insecure-skip-tls-verify=true"'
        sh 'sudo su - cicd-vm1 -c "/home/cicd-vm1/.minikube/cache/linux/amd64/v1.26.3/kubectl config set-context minikube --cluster=minikube --user=minikube"'
        sh 'sudo su - cicd-vm1 -c "/home/cicd-vm1/.minikube/cache/linux/amd64/v1.26.3/kubectl config use-context minikube"'
        sh 'sudo su - cicd-vm1 -c "/home/cicd-vm1/.minikube/cache/linux/amd64/v1.26.3/kubectl get pods"'
        sh 'sudo su - cicd-vm1 -c "/home/cicd-vm1/.minikube/cache/linux/amd64/v1.26.3/kubectl create namespace cicd-test2"'
        sh 'sudo su - cicd-vm1 -c "pwd"'
        sh 'sudo su - cicd-vm1 -c "/home/cicd-vm1/.minikube/cache/linux/amd64/v1.26.3/kubectl create -f  /var/lib/jenkins/workspace/ins-kubernetes-deployment_master/deployment.yaml"'
        sh 'sudo su - cicd-vm1 -c "/home/cicd-vm1/.minikube/cache/linux/amd64/v1.26.3/kubectl create -f  /var/lib/jenkins/workspace/ins-kubernetes-deployment_master/service.yaml"'
    }
      }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}
