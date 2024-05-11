pipeline {
  environment {
    registry = "nobodynow/kurs"
    registryCredential = 'docker_credentials'
    dockerImage = ''
    oldBuild=${currentBuild.previousBuild.number}
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
         // Clone GitHub repository
         git branch: 'master', url: 'https://github.com/kachanovskiy3214/kurs.git'
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          // Push Docker image to repository
            withDockerRegistry([ credentialsId: "docker_credentials", url: "https://index.docker.io/v1/" ]) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Cleaning Up'){
      steps{
         script {
            sh 'docker rm -f $registry:$oldBuild'
         }
      }
    }
    stage('Run Docker image') {
      steps {
        // Run Docker image
        script {
                dockerImage.run('-d -p 81:80')
        }
      }
    }
  }
}

