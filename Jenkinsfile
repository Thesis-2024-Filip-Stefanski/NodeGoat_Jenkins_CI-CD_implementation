pipeline {
  agent none
  triggers {
    pollSCM '* * * * *'
  }
  stages {
    stage('Build') {
      agent {
        docker { image 'maven:3.8.1-adoptopenjdk-11' }
      }
      steps {
        sh 'mvn --version'
      }
    }
    stage('Test') {
      agent {
        docker { image 'node:16-alpine' }
      }
      steps {
        sh 'node --version'
      }
    }
  }
}
