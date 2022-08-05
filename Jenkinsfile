pipeline {
  agent none

  stages {
    stage('Build Frontend') {
      agent {
         docker 'node:16.14.2-alpine' 
         reuseNode true
         }
      steps {
           sh 'cd jenkins-test-frontend; npm install; npm run build;'
      }
    }
    stage('Build Backend1') {
      agent any
      steps {
        sh 'docker ps'
      }
    }
    stage('Build Backend2') {
      agent { docker 'gradle:7.5-jdk11' }
      steps {
        sh 'gradle -v'
      }
    }
  }
}