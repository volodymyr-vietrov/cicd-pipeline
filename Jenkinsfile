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
        ssh '''
          npm test
        '''
      }
    }
    stage('Build Image') {
      when {
        branch "main"
      }
      steps {
        ssh '''
          docker build -t nodemain:v1.0
        '''
      }
      when {
        branch "dev"
      }
      steps {
        ssh '''
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
