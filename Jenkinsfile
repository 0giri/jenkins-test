pipeline {
  agent any

  stages {

    stage('Build Frontend') {
      steps {
        script {
          app = docker.build("teample-frontend-web")
        }
          //  sh 'cd jenkins-test-frontend; npm install; npm run build;'
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