pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/jaafargaber/testEclipseGitGithub4'

                // Run Maven on a Unix agent.
                //sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                 bat "mvn  -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        stage ('Nexus'){
            steps {
                nexusArtifactUploader(
                    credentialsId: 'NexusAdmin',
                    groupId: '${POM_GROUPID}',
                    nexusUrl: 'localhost:8081/',
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    repository: 'RepoDeploiement',
                    version: '${POM_VERSION}',
                    artifacts: [
                        [artifactId: '${POM_ARTIFACTID}',
                        classifier: '',
                        file: 'target/${POM_ARTIFACTID}-${POM_VERSION}.${POM_PACKAGING}', 
                        type: '${POM_PACKAGING}']
                    ]
                )
            } 
        }        
    }
}
