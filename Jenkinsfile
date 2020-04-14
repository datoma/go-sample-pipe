// this will start an executor on a Jenkins agent with the docker label
pipeline {
  agent any
  stages {
    stage('build base image') {
      steps {
        sh 'make build-base'
      }
    }
    stage('run tests') {
      steps {
        sh 'make build-test'
        sh 'make test-unit'
        sh 'cp /tmp/jenkins_share/gosamplepipe/report/report.xml report/'
        junit 'report/report.xml'
      }
    }
    stage('build image') {
      steps {
        sh 'make build'
      }
    }
  }
  post {
    always {
      sh 'make cleanup'
    }
  }
}
