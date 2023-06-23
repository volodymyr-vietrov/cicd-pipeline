pipeline {
  agent any
  tools {
    nodejs "node 7.8.0"
  }
  options {
    buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')
  }
  stages {
    stage('Build') {
      steps {
        sh '''
          npm install --save-dev cross-env
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
    stage('Docker Build - main') {
      when {
        branch "main"
      }
      steps {
        sh '''
          docker build -t nodemain:v1.0 .
        '''
      }
    }
    stage('Docker Build - dev') {
      when {
        branch "dev"
      }
      steps {
        sh '''
          docker build -t nodedev:v1.0 .
        '''
      }
    }
    stage('Deploy - main') {
      when {
        branch "main"
      }
      steps {
        sh '''
          docker run -d -p 3000:3000 nodemain:v1.0
        '''
      }
    }
    stage('Deploy - dev') {
      when {
        branch "dev"
      }
      steps {
        sh '''
          docker run -d -p 3001:3000 nodedev:v1.0
        '''
      }
    }
  }
}
