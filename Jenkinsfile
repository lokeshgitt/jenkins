pipeline {
   agent any
   stages {
        stage('Checkout') {
            steps {
                git branch: '${params.branch}', credentialsId: 'githubcred', url: 'https://github.com/lokeshgitt/jenkins'
                sh "ls -lart ./*"
            }
        }     
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        } 
   }    
}
