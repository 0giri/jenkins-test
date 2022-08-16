pipeline {
  agent any
  stages {
    stage('Build & Deploy All Project') {
      parallel {
        stage('Frontend') {
          environment {
            IMAGE_NAME = '0giri/frontend'
          }
          steps {
            dir(path: 'jenkins-test-frontend') {
              sh '''
              npm install; 
              npm run build;
              '''
              script {
                DOCKER_IMAGE = docker.build("${IMAGE_NAME}")
                docker.withRegistry('https://registry.hub.docker.com', "Docker-Hub") {
                  DOCKER_IMAGE.push("${env.BUILD_ID}")
                }
              }

            }

          }
        }

        stage('Backend1') {
          environment {
            IMAGE_NAME = '0giri/back1'
          }
          steps {
            dir(path: 'jenkins-pipeline/back1') {
              sh 'gradle bootJar'
              dir(path: 'build/libs') {
                script {
                  DOCKER_IMAGE = docker.build("${IMAGE_NAME}", "../..");
                  docker.withRegistry('https://registry.hub.docker.com', "Docker-Hub") {
                    DOCKER_IMAGE.push("${env.BUILD_ID}")
                  }
                }

              }

            }

          }
        }

        stage('Backend2') {
          environment {
            APP = 'test-backend2'
          }
          steps {
            dir(path: 'jenkins-pipeline/back2') {
              sh 'gradle bootJar'
            }

          }
        }

      }
    }

  }
  tools {
    nodejs 'node-16.14.2'
    gradle 'gradle-7.5.1'
  }
  environment {
    CI = 'false'
    IMAGE_REGISTRY = 'localhost'
    JENKINS_TAR_DIR = '/root/teample'
    SERVER_IP = '192.168.16.37'
    SERVER_USER = 'giri'
    SERVER_PWD = 'openbase'
    SERVER_TAR_DIR = '/home/giri/teample'
    SERVER_K8S_DIR = '/opt/obapps/teample'
    DOCKER_IMAGE = ''
  }
  parameters {
    string(name: 'RELEASE_VER', defaultValue: '0.0.0', description: '목표 릴리즈 버전')
  }
}