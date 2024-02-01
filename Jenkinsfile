node{

  properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
  
  echo "Build number is: ${env.BUILD_NUMBER}"
  echo "job name is: ${env.JOB_NAME}"
  echo "Node name is : ${env.NODE_NAME}"
  echo "Jenkins home directory is: {env.JENKINS_HOME}"
  echo "build url is: ${env.BUILD_URL}"
   
  def mavenHome = tool name: 'maven3.9.5'
  
  stage('CheckOutCode'){
  git branch: 'development', credentialsId: '2aeb9b09-28ec-4788-abc6-a1c884c78294', url: 'https://github.com/surendra-dev-projects23/maven-web-application.git'
  }
  stage('Build'){
  sh "${mavenHome}/bin/mvn clean package"
  }
  stage('ExecuteSonarQubeReport'){
  sh "${mavenHome}/bin/mvn clean sonar:sonar"
  }
  stage('UploadArtifactIntoNexus'){
  sh "${mavenHome}/bin/mvn clean deploy"
  }
  stage('DeployAppIntoTomcat'){
  sshagent(['56571939-0671-401a-86c7-7fad7cb6f801']) {
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.37.173:/opt/apache-tomcat-9.0.83/webapps"
  }
  }
  
 }
