pipeline {
  agent {
    docker {
      image 'gradle:7.5-jdk11'
    }

  }
  stages {
    stage('error') {
      steps {
        sh 'gradle -v'
      }
    }

  }
}