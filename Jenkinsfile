pipeline {
  agent none

  stages {

    stage('Build Frontend') {
      agent { docker 'node:16.14.2' }
      steps {
        script {
          dir("jenkins-test-frontend") {
            sh 'npm install; npm run build;'
            app = docker.build("jenkins-test-frontend")
          }
        }
      }
    }

    stage('Build Backend1') {
      steps {
        sh 'pwd'
        sh 'ls'
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