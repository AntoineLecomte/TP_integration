pipeline {
    agent any

    stages {
            stage('Build') {
                steps {

                    // To run Maven on a Windows agent, use
                    bat "mvn -Dmaven.test.failure.ignore=true clean package"
                }
            }
             stage('Test Analyse') {
                steps {
                    bat 'mvn test'
                    bat 'mvn checkstyle:checkstyle'
                    bat 'mvn spotbugs:spotbugs'
                    bat 'mvn pmd:pmd' 
                }
            }   
             stage('Package') {
                steps {
                    bat 'mvn clean'
                    bat 'mvn package' 
                }
            }

            stage('Publish') {
                steps {
                    archiveArtifacts '/target/*.jar'
                }
            }
        }
         post {
            always {
                junit '**/surefire-reports/*.xml'

                recordIssues enabledForFailure: true, tools: [mavenConsole(), java(), javaDoc()]
                recordIssues enabledForFailure: true, tool: checkStyle()
                recordIssues enabledForFailure: true, tool: spotBugs()
                recordIssues enabledForFailure: true, tool: cpd(pattern: '**/target/cpd.xml')
                recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')

            }

        }
}
