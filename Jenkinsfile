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
      
      post {
          always {
              archiveArtifacts artifacts: '**/*.txt',
                   allowEmptyArchive: true,
                   fingerprint: true,
                   onlyIfSuccessful: true

          }
      }
    }

    stage('deploy') {
      steps {
        bat 'echo "deploy"'
      }
    }
    stage('deploy production') {
      steps {
        bat 'echo "deploy"'
      }
    }
    stage('Branch indexing: abort') {
          when {
              allOf {
                  triggeredBy cause: "BranchIndexingCause"
                  not { 
                      changeRequest() 
                  }
              }
          }
          steps {
              script {
                  echo "Branch discovered by branch indexing"
                  currentBuild.result = 'SUCCESS' 
                  error "Caught branch indexing..."
              }
          }
      }
    
      
  }
}
