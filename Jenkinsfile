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
              archiveArtifacts artifacts: '**/*',
                   allowEmptyArchive: true,
                   fingerprint: true,
                   onlyIfSuccessful: true

          }
      }
    }

    stage('deploy') {
      
      steps {
        bat 'echo "deploy"'
        bat 'echo "good:"'
        
        sshPublisher(

          continueOnError: false, 
          failOnError: true,
          publishers: [
            sshPublisherDesc(
              configName: 'Mahwish', 
              transfers: [
                sshTransfer(
                  cleanRemote: false, 
                  excludes: '', 
                  execCommand: '', 
                  execTimeout: 120000, 
                  flatten: false, 
                  makeEmptyDirs: false, 
                  noDefaultExcludes: false, 
                  patternSeparator: '[, ]+', 
                  remoteDirectory: 'public', 
                  remoteDirectorySDF: false, 
                  removePrefix: '', 
                  sourceFiles: '**/*')],
              usePromotionTimestamp: false, 
              useWorkspaceInPromotion: false, 
              verbose: true
            )
          ]
        )
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
