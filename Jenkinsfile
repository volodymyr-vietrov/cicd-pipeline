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
           cp /data/logo_prod.svg ./src/logo.svg
        '''
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
           cp /data/logo_dev.svg ./src/logo.svg
        '''
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
           echo "Stopping all runnign containers"
           docker kill $(docker container ls --filter ancestor=nodemain:v1.0 --format={{.ID}}) || echo Failed with code $?
           
           echo "Removing all containers"
           docker container rm $(docker container ls -a --filter ancestor=nodemain:v1.0 --format={{.ID}}) || echo Failed with code $?
        '''
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
           echo "Stopping all runnign containers"
           docker kill $(docker container ls --filter ancestor=nodedev:v1.0 --format={{.ID}}) || echo Failed with code $?
           
           echo "Removing all containers"
           docker container rm $(docker container ls -a --filter ancestor=nodedev:v1.0 --format={{.ID}}) || echo Failed with code $?
        '''
        sh '''
          docker run -d -p 3001:3000 nodedev:v1.0
        '''
      }
    }
  }
}
