pipeline {
  agent {
    docker {
      image 'node:8-alpine'
      args '-p 3000:3000'
    }
  }
  environment {
    CI = 'true'
    FIREBASE_DEPLOY_TOKEN = credentials('FIREBASE_DEPLOY_TOKEN')
  }
  stages {
    stage('Install') {
      steps {
        sh 'npm install'
      }
    }
    stage('Test') {
      steps {
        sh 'npm run test'
      }
    }
    stage('Build') {
      steps {
        sh 'npm rebuild node-sass'
        sh 'npm run build'
      }
    }
    stage('Deploy') {
      when {
        expression {"${env.GIT_BRANCH}" == "origin/master"}
      }
      steps {
        sh './node_modules/.bin/firebase deploy --token=$FIREBASE_DEPLOY_TOKEN'
      }
    }
  }
}
