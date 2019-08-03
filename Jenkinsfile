pipeline {
  environment {
    REPOSITORY=980284290314.dkr.ecr.ap-south-1.amazonaws.com
    registryCredential = 'Amazon ECR Registry:Aws-AP_SOUTH_1'
    IMAGE=esther-auditplus-site
  }
  options {
    disableConcurrentBuilds ()
  }
  agent any
  tools { nodejs "node" }
  stages {
    stage('Cloning Git') {
      steps {
        git 'https://github.com/estherjeba/hellodocker.git'
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
          docker build -f ./Dockerfile -t ${REPOSITORY}/${IMAGE} 
        }
      }
    }
    stage('Deploy Image') {
      steps{
         script {
            docker.withRegistry( '', registryCredential ) {
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
