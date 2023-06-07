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
                sh "sudo ssh -T -i /home/azureuser/.ssh/VMJenk_key.pem azureuser@51.103.46.201"
                sh "sudo docker run -d -p 80:80 -h apptestjkb nevii/apptestjk:latest"
            }
        }
    }
}

