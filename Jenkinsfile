pipeline {
    environment {
        appImage = ""
    }
    agent any

    stages {
        stage('checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/lfer31/httpd-server-demo.git'
            }
            
        }
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

