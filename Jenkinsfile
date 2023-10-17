pipeline {
  agent {
    docker { image 'docker:latest' }
  }
  triggers {
    pollSCM '* * * * *'
  }
  stages {
    stage('Build') {
      steps {
        sh 'ls'
        sh 'pwd'
        sh 'docker ps'
        sh 'docker network create mynetwork'
        sh 'docker-compose build'
        sh 'docker-compose up --detach '
        sh 'docker ps '
        sh 'docker network connect mynetwork docker-compose_application_web_1'
        sh 'docker network connect mynetwork docker-compose_application_mongo_1'
        sh 'docker network inspect mynetwork'
      }
    }
    stage('Test') {
      steps {
        sh 'docker ps'
      }
    }
  }
}
