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

  environment {
    CI='false'
    IMAGE_REGISTRY='localhost'
    JENKINS_TAR_DIR='/root/teample'
    SERVER_IP='192.168.16.37'
    SERVER_USER='giri' 
    SERVER_PWD='openbase'
    SERVER_TAR_DIR='/home/giri/teample'
    SERVER_K8S_DIR='/opt/obapps/teample'
    DOCKER_REGISTRY = 'localhost'
    DOCKER_IMAGE = ''
  }

  stages {

    stage('Prepare') {
        steps {
            sh 'echo "Clonning Repository"'
            git branch: 'main',
                url: 'https://github.com/0giri/jenkins-test.git',
                credentialsId: 'github-jenkins-token'
        }
        post {
            success { 
                  sh 'echo "Successfully Cloned Repository"'
              }
              failure {
                  sh 'echo "Fail Cloned Repository"'
              }
        }
    }

    stage('Build & Deploy All Project') {

      parallel {

        stage('Frontend') {
          environment {
            APP='test-frontend'
          }
          steps {
            dir("jenkins-test-frontend") {
              sh """
              npm install; 
              npm run build;
              """

              script {
                DOCKER_IMAGE = docker.build DOCKER_REGISTRY
              }
              // docker build -t ${IMAGE_REGISTRY}/${APP}:v${RELEASE_VER}.${BUILD_TIMESTAMP} .;
              // sh 'docker save -o ${JENKINS_TAR_DIR}/${APP}.tar ${IMAGE_REGISTRY}/${APP}:v${RELEASE_VER}.${BUILD_TIMESTAMP}'
              // sh 'scp ${JENKINS_TAR_DIR}/${APP}.tar ${SERVER_USER}@${SERVER_IP}:${SERVER_TAR_DIR}'
            }
          }
        }

        stage('Backend1') {
          environment {
            APP='test-backend1'
          }
          steps {
            dir("jenkins-pipeline/back1") {
              sh 'gradle clean build'
              dir("build/libs") {
                sh 'docker build -f ../../Dockerfile -t ${IMAGE_REGISTRY}/${APP}:v${RELEASE_VER}.${BUILD_TIMESTAMP} .'
                sh 'docker save -o ${JENKINS_TAR_DIR}/${APP}.tar ${IMAGE_REGISTRY}/${APP}:v${RELEASE_VER}.${BUILD_TIMESTAMP}'
                sh 'scp ${JENKINS_TAR_DIR}/${APP}.tar ${SERVER_USER}@${SERVER_IP}:${SERVER_TAR_DIR}'

                ssh ${SERVER_USER}@${SERVER_IP} "echo ${SERVER_PWD} | su -c 'podman image load -i ${SERVER_TAR_DIR}/${APP}.tar; \
                sed -i 's/${APP}:v.*/${APP}:v${RELEASE_VER}.${BUILD_TIMESTAMP}/g' ${SERVER_K8S_DIR}/${APP}.yaml'"
                kubectl set image deployment/${APP} ${APP}=${IMAGE_REGISTRY}/${APP}:v${RELEASE_VER}.${BUILD_TIMESTAMP}
              }
            }
          }
        }

        stage('Backend2') {
          environment {
            APP='test-backend2'
          }
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