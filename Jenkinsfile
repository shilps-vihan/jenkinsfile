pipeline{
    agent any

    environment{
        PATH = "/opt/apache-maven-3.8.1/bin:$PATH"
    
    }
     stages{
         stage("clone the code"){
             steps{
                 git 'https://github.com/ravdy/hello-world.git'
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
                aws s3 cp /var/lib/jenkins/workspace/pipeline-demo/webapp/target/webapp.war s3://s3-jenkinsfile
                """
             }
         }
         stage("place s3 file to tomcat"){
             steps{
                sh """
                ./opt/apache-tomcat-8.5.65/bin/shutdown.sh
                aws s3 cp s3://s3-jenkinsfile/webapp.war /opt/apache-tomcat-8.5.65/webapps
                ./opt/apache-tomcat-8.5.65/bin/startup.sh
                """
             }
         }
     } 
     }
