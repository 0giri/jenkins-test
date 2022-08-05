pipeline {
  agent any

  stages {

    stage('Build Frontend') {
      steps {
        script {
          dir("jenkins-test-frontend") {
            sh 'cd jenkins-test-frontend; npm install; npm run build;'
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