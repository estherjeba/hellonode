pipeline {
  environment {
    VERSION = 'latest'
    ECRURL = 'http://980284290314.dkr.ecr.ap-south-1.amazonaws.com'
    ECRCRED = 'ecr:ap-south-1:b994d093-c000-4991-bbc7-906d7f11abc0' 
    PROJECT = 'esther-auditplus-site'
    IMAGE = 'esther-auditplus-site:latest'
  }
  options {
    disableConcurrentBuilds ()
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
          docker.build IMAGE
        }
      }
    }
    stage('Deploy Image') {
      steps{
         script {
            docker.withRegistry( ECRURL,ECRCRED ) {
            docker.image(IMAGE).push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $IMAGE"
      }
    }
  }
}
