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
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }  
        stage ('Deploy') 
        {
            when 
            {

                branch 'master'
            }
            steps
            { 
            sh 'ssh root@18.217.119.24 rm -rf /var/www/temp_deploy/dist/'
            sh 'ssh root@18.217.119.24 mkdir -p /var/www/temp_deploy'
            sh 'scp root@18.217.119.24:/var/www/temp_deploy/dist/'
            sh 'ssh root@18.217.119.24 "rm -rf /var/www/trainSchedule.com/dist/ && mv /var/www/temp_deploy/dist/ /var/www/trainSchedule.com/"'
            }
        }      
     }
  }

