pipeline 
{
    agent any
    stages
    {
            
         stage ('provision environment') 
          {
              steps 
              {
               echo 'provision server env'
               }
          }
         stage ('Code analyse') 
          {
             steps 
             {
              echo 'Run some lints'
              }
          }
         stage ('Unit test') 
          {
            steps 
            {
              echo 'Tests will run'
             }
          }
         stage('Build') 
          {
            steps 
            {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
          }
        stage('Deploy') 
         {
            when 
            {
                branch 'master'
            }
            steps 
            {
                withCredentials([string(credentialsId: 'web_server_login')]) 
                {
                    sshPublisher
                     (
                        publishers: 
                          [
                            sshPublisherDesc
                             (
                                configName: 'deploy_server',
                                sshCredentials: 
                                [
                                    username: 'web_server_login',
                                    encryptedPassphrase: ""
                                ], 
                                transfers:
                                [
                                    sshTransfer
                                     (
                                        sourceFiles: 'dist/trainSchedule.zip',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/tmp'
                                        
                                     )
                                ]
                              )
                           ]
                      )
                }
            }
         }     
     }
  }
