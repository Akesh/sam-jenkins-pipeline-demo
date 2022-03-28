pipeline {
  agent any
  environment {
    AWS_REGION = "us-east-1"
    ENVIRONMENT = "${params.ENVIRONMENT}"
    REPO = "${params.REPO}"
    FUNCTION = "${params.FUNCTION}"    
    LOWERCASE_FUNCTION = "${params.FUNCTION}".toLowerCase()
    STACK_NAME = "${LOWERCASE_FUNCTION}" + "-" + "stack"    
  }
  stages {
    stage('Install sam-cli') {
      steps {
        //sh 'python3 -m venv venv'
        sh 'python3 -m venv venv && venv/bin/pip3 install aws-sam-cli'
        stash includes: '**/venv/**/*', name: 'venv'
      }
    }
    stage('Build') {
      steps {
        unstash 'venv'
        //Read AWS SSM parameter store parameters 
        withAWSParameterStore(credentialsId: 'BlazePulsePipelineCredentials', naming: 'relative', path: "/${ENVIRONMENT}", recursive: true, regionName: "${AWS_REGION}") {
          sh 'venv/bin/sam build'
          stash includes: '**/.aws-sam/**/*', name: 'aws-sam'
          echo "PORTALADMIN_URL- ${PORTALADMIN_URL}"
          echo "INFRASERVICE_URL- ${INFRASERVICE_URL}"
          //dir("${env.WORKSPACE}/hello-world") {
          //  sh "zip -qr ${FUNCTION}.zip *"
          //  sh "ls *.zip"
          //}
          //executePipeline();
        }
      }
    }
    stage('Package') {
      steps {
        unstash 'venv'
        //Read AWS SSM parameter store parameters 
        withAWSParameterStore(credentialsId: 'BlazePulsePipelineCredentials', naming: 'relative', path: "/${ENVIRONMENT}", recursive: true, regionName: "${AWS_REGION}") {
          sh 'venv/bin/sam build'
          stash includes: '**/.aws-sam/**/*', name: 'aws-sam'
          echo "PORTALADMIN_URL- ${PORTALADMIN_URL}"
          echo "INFRASERVICE_URL- ${INFRASERVICE_URL}"
          sh 'aws cloudformation package --template template.yaml --s3-bucket ${BUCKET_ARTIFACTORY} --s3-prefix ${ENVIRONMENT}/${FUNCTION}  --force-upload --output-template-file packaged-template.json'
          //dir("${env.WORKSPACE}/hello-world") {
          //  sh "zip -qr ${FUNCTION}.zip *"
          //  sh "ls *.zip"
          //}
          //executePipeline();
        }
      }
    }
    stage('Deploy') {
      steps {        
        //Read AWS SSM parameter store parameters 
        withAWSParameterStore(credentialsId: 'BlazePulsePipelineCredentials', naming: 'relative', path: "/${ENVIRONMENT}", recursive: true, regionName: "${AWS_REGION}") {
           echo "BUCKET_ARTIFACTORY- ${BUCKET_ARTIFACTORY}"
           unstash 'venv'
           unstash 'aws-sam'
           sh 'venv/bin/sam deploy --stack-name ${STACK_NAME} --parameter-overrides ParameterKey=FunctionName,ParameterValue=${FUNCTION} ParameterKey=LambdaAlias,ParameterValue=${ENVIRONMENT} ParameterKey=AutoPublishCodeSha,ParameterValue=${BUILD_ID} --force-upload --s3-bucket ${BUCKET_ARTIFACTORY} --s3-prefix ${ENVIRONMENT}/${FUNCTION} --capabilities CAPABILITY_IAM --region ${AWS_REGION}'
          //executePipeline();
        }
      }
    }
  }
}

def executePipeline() {
  print('Executing pipeline for {} env'.format(BASE_URL))
}