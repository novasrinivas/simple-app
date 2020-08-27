pipeline{
    agent any
    tools{
       maven "maven"
    } 
       stages{
        stage("git checkout"){
            steps{
            git 'https://github.com/novasrinivas/simple-app'
            }
        }
        stage("maven compile"){
            steps{
                sh "mvn clean package"
            }
        }
        stage("deploy to tomcat"){
            steps{
              sshagent(['jenkins-pipeline']) {
              sh """
              
              ssh -o StrictHostKeyChecking=no ec2-user@172.31.34.249 sudo rm -Rf apache-tomcat-8.5.57.* tomcat8
              
              ssh -o StrictHostKeyChecking=no ec2-user@172.31.34.249 sudo wget https://downloads.apache.org/tomcat/tomcat-8/v8.5.57/bin/apache-tomcat-8.5.57.tar.gz
              
              ssh -o StrictHostKeyChecking=no ec2-user@172.31.34.249 sudo tar -xf apache-tomcat-8.5.57.tar.gz
              
              ssh -o StrictHostKeyChecking=no ec2-user@172.31.34.249 sudo mv apache-tomcat-8.5.57 tomcat8
              
              ssh -o StrictHostKeyChecking=no ec2-user@172.31.34.249 sudo chown -R ec2-user:ec2-user tomcat8/
              
              scp -o StrictHostKeyChecking=no target/*.war ec2-user@172.31.34.249:/home/ec2-user/tomcat8/webapps
              
              ssh -o StrictHostKeyChecking=no ec2-user@172.31.34.249  /home/ec2-user/tomcat8/bin/shutdown.sh
              ssh -o StrictHostKeyChecking=no ec2-user@172.31.34.249  /home/ec2-user/tomcat8/bin/startup.sh
              
             """
              }
            }  
        }
       }
}
    
