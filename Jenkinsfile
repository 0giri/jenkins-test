pipeline {
  agent none

  parameters {
      string(name : 'RELEASE_VER', defaultValue : '0', description : '')
  }

  stages {
    stage('Parallel Stage') {
      
      parallel {

        stage('Build Frontend') {
          agent { docker 'node:16.14.2' }
          steps {
            dir("jenkins-test-frontend") {
              sh 'npm install; npm run build;'
            }
          }
        }

        stage('Build Backend1') {
          agent { docker 'adoptopenjdk/openjdk11' }
          steps {
            dir("jenkins-pipeline/back1") {
                sh './gradlew clean build'
                sh 'ls build/libs'
                archiveArtifacts artifacts: 'build/libs/*.jar', fingerprint: true
            }
          }
        }

        stage('Build Backend2') {
          agent { docker 'adoptopenjdk/openjdk11' }
          steps {
            dir("jenkins-pipeline/back2") {
              sh './gradlew clean build'
              sh 'ls build/libs'
              sh 'docker build -t ${BUILD_TIMESTAMP} .'
            }
          }
        }

      }

    }

  }

}