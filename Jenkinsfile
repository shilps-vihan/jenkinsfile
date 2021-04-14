pipeline{
    agent any

    environment{
        PATH = "/opt/apache-maven-3.8.1/bin:$PATH"
    
    }
     stages{
         stage("clone the code"){
             steps{
                 git 'some_git_location'
             }
         }     
         stage("maven build"){
             steps{
                sh "mvn clean install"
             }
         }
         stage("upload war to s3"){
             steps{
                sh """
                aws s3 cp /var/lib/jenkins/workspace/${params.project}/webapp/target/webapp.war <some_s3 bucket>
                """
             }
         }
         stage("place s3 file to tomcat"){
             steps{
                sh """
                ./opt/apache-tomcat-8.5.65/bin/shutdown.sh
                aws s3 cp s3://<some_s3 bucket>/webapp.war /opt/apache-tomcat-8.5.65/webapps
                ./opt/apache-tomcat-8.5.65/bin/startup.sh
                """
             }
         }
     } 
     }