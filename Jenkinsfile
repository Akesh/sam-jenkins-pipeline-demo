pipeline {
  agent any
  environment {
      ENVIRONEMENT='${params.ENVIRONMENT}'
      REPO='${params.REPO}'
      FUNCTION='${params.FUNCTION.toLowerCase}'
  }
  stages {
    stage('Install sam-cli') {
      steps {
        sh 'python3 -m venv venv && venv/bin/pip3 install aws-sam-cli'
        stash includes: '**/venv/**/*', name: 'venv'
      }
    }
    stage('Parameters') {
   		steps {
		       echo "Deploying on ${params.ENVIRONMENT} on ${env.JENKINS_URL}"
		       echo "Environment=${ENVIRONMENT}"
		   }
    }
  }
}