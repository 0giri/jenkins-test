pipeline {
  agent any
  tools { [
    nodejs "node-16.14.2",
    gradle "gradle-7.5.1" 
  ] }

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
              script {
                app1 = docker.build("test-frontend:$BUILD_NUMBER")
              }
            }
          }
        }

        stage('Build Backend1') {
          steps {
            dir("jenkins-pipeline/back1") {
              sh 'gradle clean build'
              script {
                app2 = docker.build("test-backend1:$BUILD_NUMBER")
              }
            }
          }
        }

        stage('Build Backend2') {
          steps {
            dir("jenkins-pipeline/back2") {
              sh 'gradle clean build'
              script {
                app3 = docker.build("test-backend2:$BUILD_NUMBER")
              }
            }
          }
        }

      }

    }

  }

}