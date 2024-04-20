pipeline {
    agent any
    
    stages {
        stage('Fetch JUnit Dependency Details') {
            steps {
                script {
                    // Read repo.txt file
                    def repoList = readFile('repo.txt').readLines()
                    
                    // Iterate over each repository
                    for (String repo : repoList) {
                        // Clone the repository
                        def repoName = repo.tokenize('/')[-1]
                        checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: repo]]])
                        
                        // Fetch dependency details from pom.xml
                        def pomPath = "${repoName}/pom.xml"
                        def dependencyDetails = sh(script: "mvn dependency:tree -f ${pomPath} -DoutputType=xml", returnStdout: true).trim()
                        
                        // Parse XML to extract JUnit dependency details
                        def dependencyXml = new XmlSlurper().parseText(dependencyDetails)
                        def junitDependencies = dependencyXml.dependency.findAll { dep ->
                            dep.artifactId.text() == 'junit'
                        }.collect { dep ->
                            [
                                groupId: dep.groupId.text(),
                                artifactId: dep.artifactId.text(),
                                version: dep.version.text(),
                                scope: dep.scope.text()
                            ]
                        }
                        
                        // Convert JUnit dependencies to JSON and write to file
                        def jsonOutput = groovy.json.JsonOutput.toJson([junitDependencies: junitDependencies])
                        writeFile file: "${repoName}_junit_dependencies.json", text: jsonOutput
                    }
                }
            }
        }
    }
}
