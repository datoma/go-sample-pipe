// this will start an executor on a Jenkins agent with the docker label
pipeline {
  environment {
    registry = "https://nexus.blackboards.de/repository/reg-docker"
    registryCredential = "jenkins-nexus"
  }
  agent any
  stages {
    stage('build base image') {
      steps {
        sh 'make build-base'
      }
    }
    stage('Sonarqube') {
      environment {
        scannerHome = tool 'SonarScanner'
      }
      steps {
        withSonarQubeEnv('Sonar') {
          sh "${scannerHome}/bin/sonar-scanner"
        }
        timeout(time: 10, unit: 'MINUTES') {
          waitForQualityGate abortPipeline: true
        }
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
    //stage('Deploy Image') {
    //  steps{    
    //    script {
    //      docker.withRegistry( registry, registryCredential ) {
    //        dockerImage.push()
    //      }
    //    }
    //  }
    //}
  }
  post {
    always {
      sh 'make cleanup'
    }
  }
}
