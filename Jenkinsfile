node{

def mavenHome = tool name: "maven3.8.6"

echo "Jenkins url is: ${env.JENKINS_URL}"
echo "Node Name is: ${env.NODE_NAME}"
echo "Job name is: ${env.JOB_NAME}"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

stage('CheckOutCode'){
git branch: 'development', credentialsId: '30811a1e-282f-444e-99c3-9cd451f0e2c3', url: 'https://github.com/rupalikamble24/maven-web-application.git'
}

stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage('UploadArtifactsIntoArtifactoryRepo'){
sh "${mavenHome}/bin/mvn clean deploy"
}

stage('DeployAppIntoTomcatServer'){
sshagent(['101a9631-4ae3-481a-ba63-00aa6ec69fb3']) {
 sh "scp -o StrictHostKeyChecking=no  target/maven-web-application.war ec2-user@172.31.34.212:/opt/apache-tomcat-9.0.65/webapps/"
}
}

}
