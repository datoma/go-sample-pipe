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
        sh 'ls'
        sh 'ls /tmp/jenkins_share/gosamplepipe/report/'
        sh 'which junit'
        sh 'junit --version'
        junit '/tmp/jenkins_share/gosamplepipe/report/report.xml'
      }
    }
    stage('build image') {
      steps {
        sh 'make build'
      }
    }
  }
}
