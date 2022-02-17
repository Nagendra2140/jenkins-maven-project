pipeline {
    agent any
    stages {
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
        stage('Test') {
            steps {
                sh 'mvn -f hello-app/pom.xml test'
            }
            post {
                always {
                    junit 'hello-app/target/surefire-reports/*.xml'
                }
            }
        }
        stage("deploy"){
       steps{
          sshagent(['my-ssh-key']) {
          sh """
          scp -o StrictHostKeyChecking=no target/myweb.war  
          tomcat@3.109.32.250:/opt/bitnami/tomcat/webapps/
          ssh tomcat@3.109.32.250 /opt/bitnami/tomcat/bin/shutdown.sh
          ssh tomcat@3.109.32.250 /opt/bitnami/tomcat/bin/startup.sh
           """
            }
          }
        }
      }    
}
