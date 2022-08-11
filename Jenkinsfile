pipeline {
  agent any
  tools { 
    nodejs 'node-16.14.2'
    gradle 'gradle-7.5.1'
  }

  parameters {
      string(
        name : 'RELEASE_VER', 
        defaultValue : '0.0.0', 
        description : '목표 릴리즈 버전'
      )
  }

  stages {

    stage('Build Source Code') {

      parallel {

        stage('Build Frontend') {
          environment {
            CI='false'
            APP='test-frontend'
            GRADLE_VER_NAME='gradle7.5'
            IMAGE_REGISTRY='localhost'
            JENKINS_TAR_DIR='/root/teample'
            SERVER_IP='192.168.16.37'
            SERVER_USER='giri' 
            SERVER_PWD='openbase'
            SERVER_TAR_DIR='/home/giri/teample'
            SERVER_K8S_DIR='/opt/obapps/teample'
          }
          steps {
            dir("jenkins-test-frontend") {
              sh '''
                npm install; npm run build;
                docker build -t ${IMAGE_REGISTRY}/${APP}:v${RELEASE_VER}.${BUILD_TIMESTAMP}
                docker save -o ${JENKINS_TAR_DIR}/${APP}.tar ${IMAGE_REGISTRY}/${APP}:v${RELEASE_VER}.${BUILD_TIMESTAMP}
                scp ${JENKINS_TAR_DIR}/${APP}.tar ${SERVER_USER}@${SERVER_IP}:${SERVER_TAR_DIR}
              '''
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