pipeline {
  agent none

  parameters {
      string(
        name : 'RELEASE_VER', 
        defaultValue : '0.0.0', 
        description : '목표 릴리즈 버전'
      )
  }

  
  stages('All') {
    stage('Build Source Code') {
      parallel {
        stage('Build Frontend') {
          agent { docker 'node:16.14.2' }
          steps {
            dir("jenkins-test-frontend") {
              sh 'npm install; npm run build;'
              stash includes: 'build/', name: 'frontend-build'
            }
          }
        }

        stage('Build Backend1') {
          agent { docker 'adoptopenjdk/openjdk11' }
          steps {
            dir("jenkins-pipeline/back1") {
                sh './gradlew clean build'
                sh 'ls build/libs'
                stash includes: 'build/libs', name: 'backend1-build'
            }
          }
        }

        stage('Build Backend2') {
          agent { docker 'adoptopenjdk/openjdk11' }
          steps {
            dir("jenkins-pipeline/back2") {
              sh './gradlew clean build'
              sh 'ls build/libs'
              stash includes: 'build/libs', name: 'backend2-build'
            }
          }
        }
      }
    }
    stage('Build Image') {
      parallel {
        stage('Build Backend1 Image') {
          agent any 
          steps {
            unstash 'backend1-build'
            sh 'ls'
          }
        }

        stage('Build Frontend Image') {
          agent any 
          steps {
            unstash 'frontend-build'
            sh 'ls'
          }
        }

        stage('Build Backend2 Image') {
          agent any 
          steps {
            unstash 'backend2-build'
            sh 'ls'
          }
        }
      }
    }
  }

}
