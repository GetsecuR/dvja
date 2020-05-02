pipeline {
  agent any
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Ínitialize') {
      steps {
        sh '''
                echo "PATH = ${PATH}"
                echo "M2_HOME = ${M2_HOME}"
           '''
       }
     }
    stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehod --json https://github.com/GetsecuR/dvja.git > trufflehog'
        sh 'cat trufflehog'
      }
    stage ('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage ('Deploy-To-Tomcat') {
      steps {
        sshagent (['tomcat']) {
          sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@3.7.110.201:/home/ubuntu/prod/apache-tomcat-8.5.54/webapps/dvja.war'
        }
      }
   }
 }
}

