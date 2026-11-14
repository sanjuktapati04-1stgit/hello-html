pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Validate') {
      steps {
        sh 'test -f index.html'
        sh 'grep -q "<h1>" index.html'
      }
    }
    stage('Build') {
      steps {
        sh 'mkdir -p build && cp index.html build/'
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