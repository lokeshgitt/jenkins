pipeline {
   agent any
   environment {
      artifactory_url = "a"
      artifactory_creds = "b"
      sonarhosturl = "c"
      ANYPOINT_USERNAME = "lokeshdevops"
      ANYPOINT_PASSWORD = "Cloudhub_password9"
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
                sh 'zip -r output.zip /var/lib/jenkins/workspace/maven/target/*.jar'
                sh 'ls -lrta'
            }
        } 
  //        stage('SonarQube Scan') {
  //           steps {
  //             sh 'mvn sonar:sonar \
  // -Dsonar.projectKey=key \
  // -Dsonar.host.url=http://54.147.83.243:9000 \
  // -Dsonar.login=0f91b47a708948c438cdf47b3307f13c57fbad22'
  //           }
  //       }
         stage('Upload ZIP to Anypoint') {
            steps {
               script {
                    def anypointCliCommand = """
                        anypoint-cli-v4 --username ${ANYPOINT_USERNAME} --password ${ANYPOINT_PASSWORD}  \
                        runtime-mgr cloudhub-application deploy test \
                        --environment DESIGN --workers 1 \
                        --region US east /var/lib/jenkins/workspace/output.zip
                    """
                    sh anypointCliCommand
                }
            }
        }
   }    
}
