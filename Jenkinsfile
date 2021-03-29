node(){
    
    
    def MavenHome= tool name: 'M2_HOME', type: 'maven'
    properties([
    buildDiscarder(logRotator(numToKeepStr: '5')),
    pipelineTriggers([
        pollSCM('* * * * *')
                    ])
    ])
    
      echo "GitHub BranhName ${env.BRANCH_NAME}"
      echo "Jenkins Job Number ${env.BUILD_NUMBER}"
      echo "Jenkins Node Name ${env.NODE_NAME}"
  
      echo "Jenkins Home ${env.JENKINS_HOME}"
      echo "Jenkins URL ${env.JENKINS_URL}"
      echo "JOB Name ${env.JOB_NAME}"
    stage("checkout code"){
        git credentialsId: 'amitkpradhan', url: 'https://github.com/amitkpradhanorg/maven-web-application.git'
    }
    stage("build"){
        sh "${MavenHome}/bin/mvn clean package"
    }
    stage("Run Sonar Report"){
        sh "${MavenHome}/bin/mvn sonar:sonar"
    }
    stage("Upload build Artifact on Nexus"){
        sh "${MavenHome}/bin/mvn deploy"
    }
    stage("Deploy to tomcat"){
        sshagent(['3127b50c-6f23-4e86-b2fe-0b37e72626bc']) {
            sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.44.147:/opt/apache-tomcat-9.0.44/webapps/maven-web-application.war"
}
    }
    stage("Email Notification"){
        emailext body: '''Hi Team,

        Deployment is successfull, please perform sanity check.

        Thanks and regards
        Amit''', subject: 'Maven-Web-Application', to: 'amit.k.prd@gmail.com'
    }
}