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

   stage  ('check-git-secret'){
          steps {
            sh 'rm trufflehog || true'
            sh 'docker run gesellix/trufflehog --json https://github.com/satyagit-hub/Webapps.git > trufflehog'
            sh 'cat trufflehog'
          }
     
   }


   /* stage ('Source Composition Analysis'){
      steps {
        sh 'rm owasp* || true'
        sh 'wget "https://raw.githubusercontent.com/satyagit-hub/Webapps/refs/heads/master/owasp-dependency-check.sh"'
        sh 'chmod +x owasp-dependency-check.sh'
        sh 'chmod a+w /var/lib/jenkins/OWASP-Dependency-Check'
        sh 'bash owasp-dependency-check.sh'
        sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/odc-reports/dependency-check-report.xml'
      }
    }*/
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
    post {
        // Clean after build
        always {
            cleanWs(cleanWhenNotBuilt: false,
                    deleteDirs: true,
                    disableDeferredWipeout: true,
                    notFailBuild: true,
                    patterns: [[pattern: '.gitignore', type: 'INCLUDE'],
                               [pattern: '.propsfile', type: 'EXCLUDE']])
        }
    }
  }
}  
