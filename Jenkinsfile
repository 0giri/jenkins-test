pipeline {
  agent any
  tools { nodejs "node-16.14.2" , gradle 'gradle-7.5.1'}

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
          steps {
            dir("jenkins-test-frontend") {
              sh 'npm install; npm run build;'
              app = docker.build("test-frontend:$BUILD_NUMBER")
            }
          }
        }

        stage('Build Backend1') {
          agent { docker 'adoptopenjdk/openjdk11' }
          steps {
            dir("jenkins-pipeline/back1") {
              sh './gradlew clean build'
              app = docker.build("test-backend1:$BUILD_NUMBER")
            }
          }
        }

        stage('Build Backend2') {
          agent { docker 'adoptopenjdk/openjdk11' }
          steps {
            dir("jenkins-pipeline/back2") {
              sh './gradlew clean build'
              sh 'ls build/libs'
              app = docker.build("test-backend2:$BUILD_NUMBER")
            }
          }
        }

      }

    }

  }

}