node
{
    def MavenHome = tool name: "maven-3.6.3"
    
        
    stage('CheckOutMode')
    {
        git branch: 'development', credentialsId: '689aee03-a25a-427a-8bff-99353f484c91', url: 'https://github.com/pss-ec-application/maven-web-application.git'
        
    }
    stage('Build')
    {
        sh "${MavenHome}/bin/mvn clean package"
    }
    stage('ExecuteSonarQubeReport')
    {
        sh "${MavenHome}/bin/mvn sonar:sonar"
    }
    stage('UploadArtifactIntoNexus')
    {
        sh "${MavenHome}/bin/mvn deploy"
    }
    stage('DeployAppIntoTomcat')
    {
        sshagent(['63f1da19-8361-4221-8868-a7e2ac492e66'])
        {
        sh "scp -o  StrictHostKeyChecking=no target/maven-web-application.war  ec2-user@13.233.118.21:/opt/apache-tomcat-9.0.39/webapps/"
}
    }
    stage('SendEmailNotification')
    {
        mail bcc: '', body: '''Build Over...

Regards,
priyanka''', cc: 'priyankaamudala@gmail.com', from: '', replyTo: '', subject: 'Build Over', to: 'amudalapriyanka49@gmail.com'
    }
    
    

}
