pipeline {
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
              echo 'Run some Tests'
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
        stage('Deploy') {
            when 
            {
                branch 'master'
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'deploy_server',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ], 
                                    transfers: [
                                        sshTransfer
                                        (
                                          sourceFiles: 'dist/trainSchedule.zip',
                                          removePrefix: 'dist/',
                                          remoteDirectory: '/tmp',
                                          execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
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

