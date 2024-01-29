pipeline {
  agent any
  stages {
    stage('Prune Docker data'){
      steps{
        script{
          try{ 
            sh 'docker stop $(docker ps -aq)'
          } catch (err){
            echo "Caught: ${err}"
          }
          sh '''       
          docker system prune -a --volumes -f
          '''
          }
        }
      }
    stage('Build') {
      steps {
        sh '''
          docker network create test_network
          docker-compose build
          docker-compose up --detach 
          docker network connect test_network $JOB_BASE_NAME'_mongo_1'
          docker network connect test_network $JOB_BASE_NAME'_web_1'
          '''
      }
    }
    stage('Start OWASP ZAP') {
      steps {
        sh '''
        docker run -d -i -t --network=test_network --name OWASPZAP -v $(pwd):/zap/wrk/:rw owasp/zap2docker-stable
        '''
      }
    }
    stage('Copy files to OWASP ZAP'){
      steps{
        sh 'docker cp OWASPZAP_scanns/NodeGoat_custom1.yaml OWASPZAP:/zap/NodeGoat_ Complete.yaml'
      }
    }
    stage('Execute the scan'){
      steps{
           sh '''
           docker exec -i OWASPZAP zap.sh -cmd -autorun /zap/NodeGoat_ Complete.yaml
           '''
      }
    }
    stage('Export reports'){
      steps{
        sh '''
        pwd
        docker cp OWASPZAP:/zap/ZAP_REPORT.html .
        docker cp OWASPZAP:/zap/ZAP_ALERT_REPORT.md .
        '''
      }
    }
  }
  post {
    always{
      archiveArtifacts artifacts: 'ZAP_*'
    }
  }
}
