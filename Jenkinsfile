pipeline {
  agent {image 'ubuntu:latest'}
  triggers {
    pollSCM '* * * * *'
  }
  stages {
    stage('Build') {
      steps {
        docker network create mynetwork
        docker-compose build
        docker-compose up --detach 
        docker ps 
        docker network connect mynetwork docker-compose_application_web_1
        docker network connect mynetwork docker-compose_application_mongo_1
        docker network inspect mynetwork
      }
    }
    stage('Test') {
      steps {
        docker ps
      }
    }
  }
}
