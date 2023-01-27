pipeline {
  agent any
  stages {
    stage('Test') {
      steps {
        bat 'echo "Test Local Test"'
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
    stage('deploy toproduction') {
      
        steps {
                input(
                    message: "Are you sure you weant to deployit in production , Yes or No?",
                    ok: "Yes",
                    submitter: "ssbostan"
                )
                echo "Deployment stage."
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
  stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/MehwishUmer/TestJenkins.git']]])
            }
        }
  repoOwner('MahwishUmer')
repository('main')
}
