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
   		steps {
		       echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
		   }
    }
  }
}