pipeline {
  agent any
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
              echo "PATH = $(PATH)"
              echo "M2_HOME = $(M2_HOME)"
           '''   
      }
    }
    stage ('build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage ('Deploy-to-tomcat'){
      steps {
        sshagent(['tomcat']) {
          sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@13.126.172.84:/prod/apache-tomcat-10.1.33/webapps/webapp.war'
        }
      }
    }
  }
}  
