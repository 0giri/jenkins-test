pipeline {
  agent any

  stages {
    stage('Parallel Stage') {

      parallel {

        stage('Build Frontend') {
          steps {
            script {
              dir("jenkins-test-frontend") {
                sh 'ls'
                sh 'npm install; npm run build;'
                app = docker.build("jenkins-test-frontend")
              }
            }
          }
        }

        stage('Build Backend1') {
          steps {
            dir("jenkins-pipeline/back1") {
                sh './gradlew clean build'
                app = docker.build("jenkins-pipeline-back1")
            }
          }
        }

        stage('Build Backend2') {
          steps {
            dir("jenkins-pipeline/back2") {
              sh './gradlew clean build'
              app = docker.build("jenkins-pipeline-back2")
            }
          }
        }

      }

    }

  }

}