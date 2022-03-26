pipeline {
  agent any
  environment {
    ENVIRONEMENT = "${params.ENVIRONMENT}"
    REPO = "${params.REPO}"
    FUNCTION = "${params.FUNCTION}".toLowerCase()
    NEWRELIC_API_KEY = credentials('newrelic-api-key')
    //USER_DETAILS = credentials('dev/user/details')
  }
  stages {
    stage('Install sam-cli') {
      steps {
        sh 'python3 -m venv venv && venv/bin/pip3 install aws-sam-cli'
        stash includes: '**/venv/**/*', name: 'venv'
      }
    }
    stage('Build') {
      steps {
        unstash 'venv'
        echo "NEWRELIC_API_KEY- ${NEWRELIC_API_KEY}"
        //echo "USER_DETAILS- ${USER_DETAILS}"
        //echo "Executing executePipeline() function for ${ENVIRONMENT}"
        //executePipeline();
      }
    }
  }
}

def executePipeline() {
  if (ENVIRONMENT == 'DEV') {
    print('Executing pipeline for DEV environment')
  } else if (ENVIRONMENT == 'PROD') {
    print('Executing pipeline for PROD environment')
  }
}