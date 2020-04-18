node
{
    def mavenHOME = tool name : "Maven 3.6.2"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), pipelineTriggers([githubPush(), pollSCM('* * * * *')])])
    stage ('Checkout')
    {
      git branch: 'development', credentialsId: '233fe9dd-ca7b-49c8-b3e8-b48522b51b1e', 
      url: 'https://github.com/HospitalityGit/maven-web-application.git'
    }
    stage ('Build')
    {
        sh "${mavenHOME}/bin/mvn clean package "
    }
   
    stage ('ExcuteSonarQubeReport')
    {
        sh "${mavenHOME}/bin/mvn sonar:sonar"
    }
    stage ('BuildArifact IntoNexus Repository')
    {
        sh "${mavenHOME}/bin/mvn deploy"
    }
    stage ('Deploy the application')
    {
   sshagent(['48f2005f-5e0c-47e9-b909-a3e597d3f52b'])
    {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.232.49.38://opt/apache-tomcat-9.0.34/webapps/"
   }
    }
    stage ('email notification')
    {
        emailext body: 'Build is completed', subject: 'Build is Over!', to: 'vishnudefeater@gmail.com'
    }
}
