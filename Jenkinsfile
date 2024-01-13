pipeline {
   agent any
   environment {
      artifactory_url = "a"
      artifactory_creds = "b"
      sonarhosturl = "c"
   }
   stages {  
      stage('echo input variables') {
               when{
                 expressions{
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
   }    
}
