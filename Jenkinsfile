pipeline{
  agent any
  stages{
    stage("Git Checkout"){
      steps{
            git credentialsId: 'github', url: 'https://github.com/Nagendra2140/jenkins-maven-project.git'
           }
          }
     stage('Build') {
            steps {
                sh 'mvn -f hello-app/pom.xml -B -DskipTests clean package'
            }
            post {
                success {
                    echo "Now Archiving the Artifacts....."
                    archiveArtifacts artifacts: '**/*.jar'
                }
            }
        }
     stage("deploy-dev"){
       steps{
          sshagent(['my-ssh-key']) {
          sh """
          scp -o StrictHostKeyChecking=no target/hello-app-1.0.war  
          ubuntu@3.109.32.250:/opt/bitnami/tomcat/webapps/
          ssh ubuntu@3.109.32.250 /opt/bitnami/tomcat/bin/shutdown.sh
          ssh ubuntu@3.109.32.250 /opt/bitnami/tomcat/bin/startup.sh
           """
            }
          }
        }
      }
    }
