pipeline {
  agent any
  environment {
      ENVIRONEMENT='${params.ENVIRONMENT}'
      REPO_URL='${params.REPO}'
      FUNCTION_NAME='${params.FUNCTION.toLowerCase()}'
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
		       echo "Deploying on ${ENVIRONEMENT} from Repository ${REPO_URL}"
		       echo "Function=${FUNCTION_NAME}"
		   }
    }
  }
}