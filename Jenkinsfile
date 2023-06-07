pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "Maven"
        //tool name: 'Maven', type: 'maven'
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
                // sh "mvn -Dmaven.test.failure.ignore=true clean package"
                sh "docker build -t apptestjk ."

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            
        }
        stage('Push') {
            steps {
                sh "docker login -u nevii --password dckr_pat_ZaOkL3fPhwN_iSvimZ8YAxjTwvk"
                sh "docker image tag apptestjk:latest nevii/apptestjk:latest"
                sh "docker image push nevii/apptestjk:latest"
            }
        }
        stage('Pull and run') {
            steps {
               sshPublisher(publishers: [sshPublisherDesc(configName: 'VMJenk', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'bash tpjenkscript.bash', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '/home/azureuser', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '/home/azureuser/**.bash')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])
            }
        }
    }
}

