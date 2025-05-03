pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        // Timeout counter starts AFTER agent is allocated
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    environment{
        def appVersion = '' //variable declaration
        //nexusUrl = 'nexus.daws78s.online:8081'
        //region = "us-east-1"
        //account_id = "315069654700"
    }
    stages {
        stage('read the version'){
            steps{
                script{
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "application version: $appVersion"
                }
            }
        }
        stage('npm install') {
            steps {
                sh """
                    npm install
                    ls -ltr
                """
            }
        }
        stage('Build'){
            steps{
                sh """
                zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
                ls -ltr
                """
            }
        }
        stage('Find ZIP Files') {
            steps {
                sh 'find "$WORKSPACE" -name "*.zip"'
            }
        }  
        
    }    

     post { 
        always { 
            echo 'I will always say Hello again!'
            //deleteDir() /* clean up our workspace */
        }
         success {
            echo 'I succeeded!'
        }
        unstable {
            echo 'I am unstable :/'
        }
        failure {
            echo 'I failed :('
        }
        changed {
            echo 'Things were different before...'
        }
    
    }
}