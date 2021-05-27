node
{
    def mavenHome = tool name: "Maven3.8.1"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    
    
stage('CheckoutCode')
{
git branch: 'development', credentialsId: '2184da9f-e5d9-4b01-a924-1046942f9384', url: 'https://github.com/Aish-0804/maven-web-application.git'
}
stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}
stage('ExecuteSonarQubeReport')
{
  sh "${mavenHome}/bin/mvn sonar:sonar"  
}
stage('UploadArtifactsintoNexus')
{
  sh "${mavenHome}/bin/mvn deploy"  
}
stage('DeployAppintoTomcatServer')
{
  sshagent(['d3be796b-7a96-4023-ac1e-de975ddff15b']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.0.27.155:/opt/apache-tomcat-9.0.45/webapps/"

} 
}
stage('SendemailNotification')
{
  emailext body: '''Build Over - Teja Devops Jenkins Project
Scripted Way

Regards 
Teja

''', subject: 'Build Over - Teja Devops Jenkins Project', to: 'tejasreesai813@gmail.com'

}

}
