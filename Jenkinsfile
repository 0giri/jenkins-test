pipeline {
  agent any

  tools { nodejs "node-16.14.2" }

  stages {
    stage('Parallel Stage') {

      parallel {

        stage('Build Frontend') {
          steps {
            dir("jenkins-test-frontend") {
              sh 'npm install; npm run build;'
              sh 'docker build -t jenkins-test-frontend .'
            }
          }
        }

        stage('Build Backend1') {
          steps {
            dir("jenkins-pipeline/back1") {
                sh './gradlew clean build'
                sh 'ls build/libs'
                sh 'docker build -t jenkins-pipeline-back1 .'
            }
          }
        }

        stage('Build Backend2') {
          steps {
            dir("jenkins-pipeline/back2") {
              sh './gradlew clean build'
              sh 'ls build/libs'
              sh 'docker build -t jenkins-pipeline-back2 .'
            }
          }
        }

      }

    }

  }

}