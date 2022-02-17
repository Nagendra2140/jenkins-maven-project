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
                    archiveArtifacts artifacts: '**/*.war'
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
        stage("deploy-dev"){
       steps{
          sshagent(['my-ssh-key']) {
          sh """
          scp -o StrictHostKeyChecking=no target/myweb.war  
          ubuntu@yourip:/opt/bitnami/tomcat/webapps
          ssh ubuntu@3.109.32.250 /opt/tomcat/bin/shutdown.sh
          ssh ubuntu@3.109.32.250 /opt/tomcat/bin/startup.sh
           """
            }
          }
        }
      }
    }
}
