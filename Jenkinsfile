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
              configName: 'MyWindows', 
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
       
      script {
            // Define Variable
             def USER_INPUT = input(
                    message: 'Do you want to deployin production - Yes or No?',
                    parameters: [
                            [$class: 'ChoiceParameterDefinition',
                             choices: ['no','yes'].join('\n'),
                             name: 'input',
                             description: 'Menu - select box option']
                    ])

            echo "The answer is: ${USER_INPUT}"

            if( "${USER_INPUT}" == "yes"){
                steps {
                  bat 'echo "deploy"'
                }
            } else {
                //do something else
            }
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
