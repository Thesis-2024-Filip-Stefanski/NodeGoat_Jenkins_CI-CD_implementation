pipeline {
   agent any
   triggers {
        pollSCM '* * * * *'
    }
    stages {
        stage('Build') {
            agent {
                docker { image 'maven:3.8.1-adoptopenjdk-11' }
              }
            steps {
                echo "Building.."
                sh 'node --version' 
            }
        }
        stage('Test') {
            agent {
                docker { image 'maven:3.8.1-adoptopenjdk-11' }
              }
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
            agent {
                docker { image 'maven:3.8.1-adoptopenjdk-11' }
              }
            steps {
                echo 'Deliver....'
                sh '''
                echo "doing delivery stuff.."
                '''
            }
        }
    }
}
