pipeline {
   agent any
   environment {
      artifactory_url = "a"
      artifactory_creds = "b"
      sonarhosturl = "c"
   }
   stages {  
      stage('echo input variables') {
         when {
            expression {
             params.env == 'prod'
         }
      }
         steps{
            sh '''
            set -x
            echo api_name: ${api_name}
            echo build type: ${buildtype}
            echo environment: ${env}
            echo build version: ${bv}
            echo isreleasecandidate: ${isrelease}
            '''
         }
      }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        } 
         stage('SonarQube Scan') {
            steps {
                script {
                    withSonarQubeEnv('sonarqube') {
                        // Other scanner options can be configured here
                        sh 'sonar-scanner -Dsonar.projectKey=key -Dsonar.sources=.'
                    }
                }
            }
        }
   }    
}
