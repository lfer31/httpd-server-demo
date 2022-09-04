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
        stage('Change yaml kubernets') {
            steps {
              script {
                sh "cat myweb.yaml"
                sh 'sed -i "s/<TAG>/v${BUILD_NUMBER}/" myweb.yaml'
                sh "cat myweb.yaml"
              }
            }
        }
        stage('Deplpy k8s') {
            steps {
              withKubeConfig([credentialsId: 'mykubeconfig']) {
                sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"'
                sh 'chmod u+x ./kubectl'
                sh './kubectl apply -f myweb.yaml'
              }
            }
        }

    }
}
