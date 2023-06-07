pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "Maven"
        // tool name: 'Maven', type: 'maven'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Nevralgie/DevopsJenkinsfilePipeline.git']])

            }
        }
        
        stage('Build') {
            steps {


                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
                sh "docker build -t apptestjk ."

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
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
        stage('Push') {
            steps {
                sh "docker login -u nevii --password dckr_pat_ZaOkL3fPhwN_iSvimZ8YAxjTwvk"
                sh "docker image tag apptestjk:latest nevii/apptestjk:latest"
                sh "docker image push nevii/apptestjk:latest"
            }
        }
    }
}
