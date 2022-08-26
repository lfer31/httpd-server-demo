stage('build') {
  node {
    def app

    stage('Clone repository') {
      checkout scm
    }

    stage('Build image') {
      app = docker.build("k3d-hub:5000/httpd-server")
    }

    stage('Test image') {
      app.inside {
        sh 'echo "Tests passed"'
      }
      sh 'docker info | grep -A 20 "Insecure Registries"'
    }

    stage('Push image') {
      docker.withRegistry('http://k3d-hub:5000', '') {
        app.push("v${env.BUILD_NUMBER}")
      }
    }
  }
}
