node {
    def mavenHome= tool name: 'maven3.9.8'
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '5', numToKeepStr: '')), pipelineTriggers([cron('* * * * *')])])
    
    echo "job name is: ${env.JOB_NAME}"
    echo "Build number is: ${env.BUILD_NUMBER}"
    echo "the node name is: ${env.NODE_NAME}"
    echo "the job url is: ${env.JOB_URL}"
    
    stage('checkout'){
    git branch: 'development', credentialsId: '8f8771f2-49ce-4c66-8898-aa62b3568b8d', url: 'https://github.com/Travelling-2026/maven-web-application.git'
    }
    stage('Build'){
        sh "$mavenHome/bin/mvn clean package"
    }
    stage('ExecuteSonarQubeReport'){
        sh  "$mavenHome/bin/mvn clean sonar:sonar"
    }
    stage('UploadArtifactsIntoNexus'){
        sh  "$mavenHome/bin/mvn clean deploy"
    }
    stage('DeployAppIntoTomcat'){
     sshagent(['28721295-1f9c-4e56-b625-394eec5bcf83']){
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.3.81:/opt/apache-tomcat-9.0.102/webapps" 
     }
    }
}
