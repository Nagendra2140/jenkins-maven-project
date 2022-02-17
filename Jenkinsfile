pipeline{
  agent any
  stages{
    stage("Git Checkout"){
      steps{
            git credentialsId: 'github', url: 'https://github.com/Nagendra2140/jenkins-maven-project.git'
           }
          }
     stage("Maven Build"){
       steps{
            sh "mvn clean package"
            sh "mv target/*.war target/myweb.war"
             }
            }
     stage("deploy-dev"){
       steps{
          sshagent(['my-ssh-key']) {
          sh """
          scp -o StrictHostKeyChecking=no target/myweb.war  
          ubuntu@3.109.32.250:/opt/bitnami/tomcat/webapps/
          ssh ubuntu@3.109.32.250 /opt/tomcat/bin/shutdown.sh
          ssh ubuntu@3.109.32.250 /opt/tomcat/bin/startup.sh
           """
            }
          }
        }
      }
    }
