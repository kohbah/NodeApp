pipeline {
  environment {
    registry = "jfrog"
    registryCredential = 'jfrog'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/kohbah/standardjenkins.git'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
           app = docker.build("nodeapp/latest")
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
        docker.withRegistry('jfrog', 'jfrog') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
              }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
