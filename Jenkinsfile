pipeline {
  agent any
  stages {
    stage('Prune Docker data'){
      steps{
        sh 'docker system prune -a --volumes -f'
      }
    }
    stage('Build') {
      steps {
        sh '''
          docker network create mynetwork
          docker-compose build
          docker-compose up --detach 
          docker ps 
          docker network connect mynetwork test_mongo_1
          docker network connect mynetwork test_web_1
          docker network inspect mynetwork
          '''
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
  post {
    always{
      sh 'docker system prune -a --volumes -f'
    }
  }
}
