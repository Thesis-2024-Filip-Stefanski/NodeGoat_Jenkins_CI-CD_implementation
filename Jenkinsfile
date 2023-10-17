pipeline {
  agent any
  stages {
    stage('Back-end') {
      steps {
        sh '''
          docker version
          docker info
          docker ps
          '''
      }
    }
    stage('Front-end') {
      agent {
        docker { image 'node:16-alpine' }
      }
      steps {
        sh 'node --version'
      }
    }
  }
}
