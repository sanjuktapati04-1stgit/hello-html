pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Validate') {
      steps {
        sh 'test -f helloworld.html'
        sh 'grep -q "<h1>" helloworld.html'
      }
    }
    stage('Build') {
      steps {
        sh 'mkdir -p build && cp helloworld.html build/'
        sh 'tar -czf site.tar.gz build'
      }
    }
    stage('Archive') {
      steps { archiveArtifacts artifacts: 'site.tar.gz', fingerprint: true }
    }
  }
  post {
    success { echo 'Build succeeded' }
    failure { echo 'Build failed' }
  }
}