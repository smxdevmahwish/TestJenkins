pipeline {
  agent any
  stages {
    stage('Test') {
      steps {
        bat 'echo "Test Local"'
      }
    }

    stage('Build') {
      steps {
        bat 'echo build'
      }
    }

    stage('deploy') {
      steps {
        bat 'echo "deploy"'
      }
    }

  }
}