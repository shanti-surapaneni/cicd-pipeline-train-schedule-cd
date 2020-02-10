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
                withCredentials([usernamePassword(credentialsId: 'web_server_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) 
                {
                    sshPublisher
                     (
                        failOnError: true,
                        continueOnError: false,
                        publishers: 
                          [
                            sshPublisherDesc

                                (
                                   configName: 'deploy_server',
                                   sshCredentials: 
                                    [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
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
