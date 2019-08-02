pipeline {
  environment {
    registry = "estherjeba/docker-test"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  tools { nodejs "node" }
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/estherjeba/hellonode.git'
      }
    }
    stage('Build') {
       steps {
         sh 'npm install'
       }
    }
    stage('Test') {
      steps {
        sh 'npm test'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry   
        }
      }
    }
    stage('Deploy Image') {
      steps{
         script {
            def version = readFile('VERSION')
            docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
            dockerImage.push(version)
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry"
      }
    }
  }
}
