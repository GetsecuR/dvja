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
        sh 'rm trufflehogout || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/GetsecuR/dvja.git > trufflehogout'
        sh 'pwd'
        sh 'whoami'
        sh 'cat trufflehogout'
      }
    }
    stage ('Source Composition Analysis') {
      steps {
         sh 'rm OWASP* || true'
         sh 'wget "https://raw.githubusercontent.com/GetsecuR/dvja/master/OWASP-dependency-check.sh" '
         sh 'pwd'
         sh 'whoami'
         sh 'chmod +x OWASP-dependency-check.sh'
         sh 'bash OWASP-dependency-check.sh'
      }
    }
    stage ('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage ('Deploy-To-Tomcat') {
      steps {
        sshagent (['tomcat']) {
          sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@15.206.91.43:/home/ubuntu/prod/apache-tomcat-8.5.54/webapps/dvja.war'
        }
      }
   }
 }
}
