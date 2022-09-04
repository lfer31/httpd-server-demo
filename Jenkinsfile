pipeline {
    environment {
        appImage = ""
    }
    agent any

    stages {
        stage('Build') {
            steps {
                // build
                script {
                    appImage = docker.build "registry31.duckdns.org/httpd-demo:v$BUILD_NUMBER"
                    echo "registry31.duckdns.org/httpd-demo:v$BUILD_NUMBER"
                }
                
            }
        }
        stage('Push') {
            steps {
                script {
                   docker.withRegistry("") {
                       appImage.push()
                   }
                }
            }
        }

    }
}

