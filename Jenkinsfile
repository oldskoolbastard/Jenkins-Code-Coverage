pipeline {
     agent any
  
     tools {nodejs "Node16"}
  
     stages {
    
    stage("Build") {
             
        steps {
                echo "Building"
                sh "npm install"
                sh "npm run build"
                 
            }
        }
        stage("Test") {
             
        steps {
                 
                echo "Testing"
                sh "npm test"
                
                 
            }
            post{
                always{
                    
                    publishCoverage adapters: [coberturaAdapter(mergeToOneReport: true, path: '**/output/coverage/jest/')], sourceFileResolver: sourceFiles('NEVER_STORE')
                }
            }
        }

        stage("Approval"){
           steps{mail to: 'imchaudhary101@gmail.com', subject: "Please approve #${env.BUILD_NUMBER}", body: "See ${env.BUILD_URL}input or for more info please click here ${env.BUILD_URL}console" 
               input "Ready to deploy for ${params.ENVIRONMENT} Server ?"
                

           } 
        }

        stage("Deploy") {

             steps{ 
                    echo "deploying for {$params.ENVIRONMENT}"
                    
                 }
                 
                 

                   /* sshPublisher(publishers: [sshPublisherDesc(configName: 'bastion', 
                    transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, 
                    makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/var/www/html', remoteDirectorySDF: false, removePrefix: '/build', 
                    sourceFiles: 'build/*')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
                    */ }
            
     
        }
    


  post {
    success {
      sh "echo 'Send mail on success'"
      mail to:"imchaudhary101@gmail.com", subject:"SUCCESS: ${currentBuild.fullDisplayName}", body: "Yay, we passed. *${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}"
    }
    failure {
          step([$class: 'Mailer',
            notifyEveryUnstableBuild: true,
            recipients: "imchaudhary101@gmail.com",
            sendToIndividuals: true])
    }
  }
}