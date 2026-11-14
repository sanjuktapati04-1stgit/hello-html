pipeline {
  agent any
  environment {
    TOMCAT_WEBAPPS_DIR = '/mnt/tomcat/webapps/ROOT'
  }
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
    stage('Deploy') {
      steps {
        sh '''
          test -d "$TOMCAT_WEBAPPS_DIR" || {
            echo "Tomcat webapps directory is not available: $TOMCAT_WEBAPPS_DIR"
            exit 1
          }
          mkdir -p deploy
          tar -xzf site.tar.gz -C deploy
          test -f deploy/build/helloworld.html
          cp -a deploy/build/. "$TOMCAT_WEBAPPS_DIR/"
        '''
      }
    }
  }
  post {
    success { echo 'Build succeeded' }
    failure { echo 'Build failed' }
  }
}