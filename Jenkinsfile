pipeline {
  environment {
    registry = "shaker242/jenkins"
    registryCredential = "dockerhub"
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/dglib/shaker242.lab.git'
      }
    }
    stage('Building image') {
      steps{
        script {
             dir("jenkins"){
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
          }
        }
      }
    }
    stage('Pushing Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
    stage('Deploy') {
      steps {
        sh "kubectl set image deployment/dlogo-deploy dlogo=$registry:$BUILD_NUMBER"
      }
    }
  }
}