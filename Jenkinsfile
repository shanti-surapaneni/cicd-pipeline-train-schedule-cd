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
        stage ('Deploy') 
        {
            when 
            {

                branch 'master'
            }
            steps
            { 
            sh 'ssh -i ../../s.pem -o StrictHostKeyChecking=no centos@ec2-18-217-119-24.us-east-2.compute.amazonaws.com rm -rf /var/www/temp_deploy/dist/'
            sh 'ssh -i ../../s.pem -o StrictHostKeyChecking=no centos@ec2-18-217-119-24.us-east-2.compute.amazonaws.com mkdir -p /var/www/temp_deploy'
            sh 'scp -i ../../s.pem -o StrictHostKeyChecking=no centos@ec2-18-217-119-24.us-east-2.compute.amazonaws.com:/var/www/temp_deploy/dist/'
            sh 'ssh -i ../../s.pem -o StrictHostKeyChecking=no centos@ec2-18-217-119-24.us-east-2.compute.amazonaws.com "rm -rf /var/www/trainSchedule.com/dist/ && mv /var/www/temp_deploy/dist/ /var/www/trainSchedule.com/"'
            }
        }      
     }
  }
