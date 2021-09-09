node
{
   def mavenhome = tool name: "maven3.8.2"
  stage('checkoutcode')
  {
     git branch: 'development', credentialsId: '56ceb286-6d79-47f4-b7f3-63880c594242', url: 'https://github.com/kumar2pavan/maven-web-application.git' 
  }
  stage('build')
  {
      sh "${mavenhome}/bin/mvn clean package"
  }
  stage('executesonarqubereport')
  {
     sh "${mavenhome}/bin/mvn sonar:sonar" 
  }
  stage('upload artifact in to nexus')
  {
     sh "${mavenhome}/bin/mvn deploy" 
  }
  stage('deployappintotaomcat')
  {
      sshagent(['6c30d1f9-daf7-4367-abf9-780f114ea036'])
      {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.227.183:/opt/apache-tomcat-9.0.52/webapps/"
      }
  }
      stage('sendemailnotification')
      {
        mail bcc: '', body: '''buildfinish

        Regards,
        pavantechnologies,
         9704594336.''', cc: 'pavankumarpkr1994@gmail.com', from: '', replyTo: '', subject: 'buildfinish', to: 'pr7404610@gmail.com'  
      }
  
  }
