pipeline {
  agent none

  stages {
    stage('Build Frontend') {
      agent { docker 'node:16.14.2-alpine' }
      steps {
        sh 'ls'
        sh 'cd jenkins-test-frontend'
        sh 'npm install'
        sh 'npm run build'
        sh 'docker build -t localhost/test:v0.1.0.20220804v1'
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