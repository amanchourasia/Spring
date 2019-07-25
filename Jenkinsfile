pipeline{
    agent any
    stages{
        /*stage('checkout'){
            steps{
                withCredentials([string(credentialsId: 'springMan', variable: 'git')]) {
                echo "My password is '${git}'!"
                checkout([$class: 'GitSCM',
                branches: [[name: 'dev']],
                extensions: [[$class: 'WipeWorkspace']],
                userRemoteConfigs: [[url: "${git}"]]
                ])
                }
            }
        }*/
        stage ('build and test'){
            steps{
                    sh "mvn clean install -DskipTests."
            }
        }
         
       /*stage('Sonar'){
            environment{
                scannerHome=tool 'sonar scanner'
            }
            steps{
                sh "mvn sonar:sonar -Dsonar.host.url=http://3.14.251.87:9000" 
            }
        }*/
         stage ('Artifact'){
             steps{
                     withCredentials([usernamePassword(credentialsId: 'manisaNexsus', passwordVariable: 'pass', usernameVariable: 'usr')]) {
                         sh 'curl -u $usr:$pass --upload-file src/target/webapp-Server-0.0.1-SNAPSHOT.war http://3.14.251.87:8081/nexus/content/repositories/devopstraining/Team_AHM/webapp-Server-0.0.1-SNAPSHOT.war'
                     }
                 }
        }
    //   stage ('Deploy'){
    //         steps{
    //              withCredentials([usernamePassword(credentialsId: 'devops-tomcat', passwordVariable: 'pass', usernameVariable: 'userId')]) {
        
    //                 sh 'curl -u  $userId:$pass http://ec2-18-224-182-74.us-east-2.compute.amazonaws.com:8080/manager/text/undeploy?path=/ManisaSpringSample'
    //                 sh  'curl -u  $userId:$pass --upload-file target/SpringBootJSP-${BUILD_NUMBER}.war http://ec2-18-224-182-74.us-east-2.compute.amazonaws.com:8080/manager/text/deploy?config=file:/var/lib/tomcat8/SpringBootJSP-${BUILD_NUMBER}.war\\&path=/ManisaSpringSample'
    //             }
    //         }
    
    //     }
    }
    post{
        success {
            slackSend (color: 'good', message: "BUILD SUCCESSFUL: The project in '${JOB_NAME} with build number [${BUILD_NUMBER}]' has successfully built")
    }
        unsuccessful{
            slackSend (color: 'danger', message: "BUILD FAIL: The project in '${JOB_NAME} with build number  [${BUILD_NUMBER}]' has failed")
        }
    // success {
    //         sendEmail("Successful");
    //     }
    //     failure {
    //         sendEmail("Failed");
    //     }
    // }

    }
}
