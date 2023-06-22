pipeline {
  agent any
  options {
    buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')
  }
  stages {
    stage('Build') {
      steps {
        sh '''
          npm install
        '''
      }
    }
    stage('Test') {
      steps {
        sh '''
          npm test
        '''
      }
    }
    stage('Build Image - main') {
      when {
        branch "main"
      }
      steps {
        sh '''
          docker build -t nodemain:v1.0
        '''
      }
    }
    stage('Build Image - dev') {
      when {
        branch "dev"
      }
      steps {
        sh '''
          docker build -t nodedev:v1.0
        '''
      }
    }
    stage('Dev only stage') {
      when {
        branch "dev*"
      }
      steps {
        sh '''
          cat README.md
        '''
      }
    }
  }
}
