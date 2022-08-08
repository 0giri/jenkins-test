pipeline {
  agent any

  stages {

    stage('Build Frontend') {
      steps {
        script {
          dir("jenkins-test-frontend") {
            cd 'ls'
            // sh 'npm install; npm run build;'
            // app = docker.build("jenkins-test-frontend")
          }
        }
      }
    }

    stage('Build Backend1') {
      steps {
        dir("jenkins-test-frontend") {
            cd 'ls'
            // sh 'npm install; npm run build;'
            // app = docker.build("jenkins-test-frontend")
      }
    }

    stage('Build Backend2') {
      dir("jenkins-pipeline/back2") {
        steps {
          sh 'ls'
          sh 'gradle -v'
        }
      }
    }

  }
}