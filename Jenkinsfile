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
    stage('Start OWASP ZAP') {
      steps {
        sh '''
        docker run -d -i -t --network=mynetwork --name OWASPZAP -v $(pwd):/zap/wrk/:rw owasp/zap2docker-stable
        docker ps
        '''
      }
    }
    stage('Copy files to OWASP ZAP'){
      steps{
        sh 'docker cp OWASPZAP_scanns/normal_scann_no_fail_on_warning.yaml OWASPZAP:/zap/normal_scann_no_fail_on_warning.yaml'
      }
    }
    stage('Execute the scan'){
      steps{
           sh '''
           docker exec -i OWASPZAP zap.sh -cmd -autorun /zap/normal_scann_no_fail_on_warning.yaml
           docker exec OWASPZAP sh -c ls
           docker exec -i OWASPZAP pwd
           docker cp OWASPZAP:/zap/ZAP_REPORT.html .
           docker cp OWASPZAP:/zap/ZAP_ALERT_REPORT.md .
           '''
      }
    }
  }
  post {
    always{
      sh 'docker system prune -a --volumes -f'
    }
  }
}
