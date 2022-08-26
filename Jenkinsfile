pipeline {

  options {
    ansiColor('xterm')
  }

  agent {
    kubernetes {
      yamlFile 'builder.yaml'
    }
  }

  stages {

    stage('Kaniko Build & Push Image') {
      steps {
        container('kaniko') {
          script {
            sh '''
            /kaniko/executor --dockerfile `pwd`/Dockerfile \
                             --context `pwd` \
                             --destination=k3d-hub:5000/httpd-server:v${BUILD_NUMBER} \
                             -insecure --skip-tls-verify
            '''
          }
        }
      }
    }
  }
}
