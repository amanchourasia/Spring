pipeline{
    agent any
    stages{
        stage ('build and test'){
            steps{
                    sh "mvn clean install"
            }
        }
         
         stage('Sonar') {
            environment {
                scannerHome=tool 'sonar scanner'
            }
            steps{
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'Hemant_Sonar_Cred', usernameVariable: 'USER', passwordVariable: 'PASS']]){
                    sh "mvn $USER:$PASS -Dsonar.host.url=http://ec2-3-17-164-37.us-east-2.compute.amazonaws.com:9000"
                }
            }
        }
        stage ('Uploading artifact to nexus'){
            steps{
                withCredentials([usernamePassword(credentialsId: 'Hemant_Nexus_Cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'curl -u $USER:$PASS --upload-file target/webapp-${BUILD_NUMBER}.war  http://ec2-3-17-164-37.us-east-2.compute.amazonaws.com:8081/nexus/content/repositories/devopstraining/Team_AHM/webapp-${BUILD_NUMBER}.war'
                }
            }
        }
      stage ('Deploy'){
             steps{
                  withCredentials([usernamePassword(credentialsId: 'devops-tomcat', passwordVariable: 'pass', usernameVariable: 'userId')]) {
        
                     sh 'curl -u  $userId:$pass http://ec2-18-188-202-13.us-east-2.compute.amazonaws.com:8080/manager/text/undeploy?path=/ManisaSpringSample'
                     sh  'curl -u  $userId:$pass --upload-file target/webapp-${BUILD_NUMBER}.war http://ec2-18-188-202-13.us-east-2.compute.amazonaws.com:8080/manager/text/deploy?config=file:/var/lib/tomcat8/webapp-${BUILD_NUMBER}.war\\&path=/ManisaSpringSample'
                 }
             }
    
         }
    }
    post{
        success {
            slackSend (color: 'good', message: "BUILD SUCCESSFUL: The project in '${JOB_NAME} with build number [${BUILD_NUMBER}]' has successfully built")
    }
        unsuccessful{
            slackSend (color: 'danger', message: "BUILD FAIL: The project in '${JOB_NAME} with build number  [${BUILD_NUMBER}]' has failed")
        }

    }
}
