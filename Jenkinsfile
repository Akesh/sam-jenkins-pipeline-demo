pipeline {
  agent any
  stages {
    stage('Install sam-cli') {
      steps {
        sh 'python3 -m venv venv && venv/bin/pip3 install aws-sam-cli'
        stash includes: '**/venv/**/*', name: 'venv'
      }
    }
    stage('Parameters') {
      when {
        expression {
          return params.ENVIRONMENT == 'DEV'
        }
      }
      steps {
        echo 'Deploying to DEV environment'
      }
      when {
        expression {
          return params.ENVIRONMENT == 'PROD'
        }
        steps {
          echo 'Deploying to PROD environment'
        }
      }
    }
  }
}