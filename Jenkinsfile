pipeline {
  agent any
  environment {
    ENVIRONEMENT = "${params.ENVIRONMENT}"
    REPO = "${params.REPO}"
    FUNCTION = "${params.FUNCTION}".toLowerCase()
    //NEWRELIC_API_KEY = credentials('newrelic-api-key')
    BASE_URL
  }
  stages {
    stage('Install sam-cli') {
      steps {
      	sh 'python3 -m venv venv'
        //sh 'python3 -m venv venv && venv/bin/pip3 install aws-sam-cli'
        stash includes: '**/venv/**/*', name: 'venv'
      }
    }
    stage('Build') {     
      steps {
        unstash 'venv'      
         withAWSParameterStore(credentialsId: 'BlazePulsePipelineCredentials', naming: 'relative', path: "/${ENVIRONEMENT}", recursive: true, regionName: 'us-east-1'){                       	                      
        	echo "Executing executePipeline() function for ${ENVIRONMENT} with base url ${BASEURL}"
        	${env.BASE_URL}="Test Value"      	        
        	//executePipeline();
      		}   
        }              
    }
  }
}


def executePipeline() {
    print('Executing pipeline for {} env'.format(BASE_URL))
}