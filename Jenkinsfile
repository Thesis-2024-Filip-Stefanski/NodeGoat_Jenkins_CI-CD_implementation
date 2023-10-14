pipeline {
    agent any
    
    triggers {
        pollSCM '* * * * *'
    }
    stages {
        stage('Build') {
            steps {
                echo "Building.."
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
                echo "Testing.."
                sh '''
                cd myapp
                python3 hello.py
                python3 hello.py --name=Brad
                '''
            }
        }
        stage('Deliver') {
            steps {
                echo 'Deliver....'
                sh '''
                echo "doing delivery stuff.."
                '''
            }
        }
    }
}
