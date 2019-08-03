pipeline {
  environment {
    VERSION = 'latest'
    ECRURL = 'http://980284290314.dkr.ecr.ap-south-1.amazonaws.com'
    registryCredential = 'Amazon ECR Registry:Aws-AP_SOUTH_1'
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
          docker.build("$IMAGE", /Dockerfile )
        }
      }
    }
    stage('Deploy Image') {
      steps{
         script {
            docker.withRegistry( ECRURL,registryCredential ) {
            docker.image(IMAGE).push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        script {
          docker rmi $IMAGE
        }
      }
    }
  }
}
