pipeline {
  agent any
  stages {
    stage('Build & Deploy All Project') {
      parallel {
        stage('Frontend') {
          environment {
            REPOSITORY = '0giri/frontend'
          }
          steps {
            dir(path: 'jenkins-test-frontend') {
              sh '''
              npm install; 
              npm run build;
              '''
              script {
                DOCKER_IMAGE = docker.build("${REPOSITORY}")
                docker.withRegistry("${REGISTRY}", "${DOCKER_CREDENTIAL}") {
                  DOCKER_IMAGE.push("${env.BUILD_ID}")
                }
              }

            }

          }
        }

        stage('Backend1') {
          environment {
            REPOSITORY = '0giri/back1'
          }
          steps {
            dir(path: 'jenkins-pipeline/back1') {
              sh 'gradle bootJar'
              dir(path: 'build/libs') {
                script {
                  DOCKER_IMAGE = docker.build("${REPOSITORY}", "../..")
                  docker.withRegistry("${REGISTRY}", "${DOCKER_CREDENTIAL}") {
                    DOCKER_IMAGE.push("${env.BUILD_ID}")
                  }
                }

              }

            }

            kubeconfig(credentialsId: 'k8s-144', serverUrl: 'https://192.168.16.141:6443', caCertificate: '') {
              sh 'kubectl get pods'
            }

          }
        }

        stage('Backend2') {
          environment {
            REPOSITORY = '0giri/back2'
          }
          steps {
            dir(path: 'jenkins-pipeline/back2') {
              sh 'gradle bootJar'
              dir(path: 'build/libs') {
                script {
                  DOCKER_IMAGE = docker.build("${REPOSITORY}", "../..")
                  docker.withRegistry("${REGISTRY}", "${DOCKER_CREDENTIAL}") {
                    DOCKER_IMAGE.push("${env.BUILD_ID}")
                  }
                }

              }

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
    REGISTRY = 'https://registry.hub.docker.com'
    DOCKER_CREDENTIAL = 'docker-hub'
  }
  parameters {
    string(name: 'RELEASE_VER', defaultValue: '0.0.0', description: '목표 릴리즈 버전')
  }
}