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
        stage('Deplpy k8s') {
            agent {
              docker { image 'joshendriks/alpine-k8s' }
            }
            steps {
              withKubeConfig([credentialsId: 'mykubeconfig']) {
                // sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"'
                // sh 'chmod u+x ./kubectl'
                sh "cat myweb.yaml"
                sh 'sed -i "s/<TAG>/v${BUILD_NUMBER}/" myweb.yaml'
                sh 'cat myweb.yaml'
                sh 'kubectl apply -f myweb.yaml --insecure-skip-tls-verify'
              }
            }
        }

    }
    post {
      success {
        mail to: lfer31@gmail.com, subject: ‘The Pipeline httpd_demo_server success :)‘
      }
      failure {
        mail to: lfer31@gmail.com, subject: ‘The Pipeline httpd_demo_server failed :(‘
      }
    }
}
