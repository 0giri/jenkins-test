pipeline {
  agent none

  stages {
    stage('Build Frontend') {
      agent { docker 'node:16.14.2-alpine' }
      steps {
        sh 'node -v'
      }
    }
    stage('Build Backend1') {
      agent { docker 'gradle:7.5-jdk11' }
      steps {
        sh 'gradle -v'
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