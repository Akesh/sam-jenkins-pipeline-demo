pipeline {
  agent any
  environment {
    AWS_REGION = "us-east-1"
    ENVIRONEMENT = "${params.ENVIRONMENT}"
    REPO = "${params.REPO}"
    FUNCTION = "${params.FUNCTION}".toLowerCase()
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
        withAWSParameterStore(credentialsId: 'BlazePulsePipelineCredentials', naming: 'relative', path: "/${ENVIRONEMENT}", recursive: true, regionName: "${AWS_REGION}") {
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
    stage('Upload Artifacts to S3') {
      steps {
        unstash 'venv'
        //Read AWS SSM parameter store parameters 
        withAWSParameterStore(credentialsId: 'BlazePulsePipelineCredentials', naming: 'relative', path: "/BUCKET", recursive: true, regionName: "${AWS_REGION}") {
          echo "ARTIFACTORY Bucket- ${ARTIFACTORY}"
          dir("${env.WORKSPACE}/hello-world") {
          //  echo "Uploading artifacts to S3 bucket"
          //  s3Upload(file: "${FUNCTION}.zip", bucket: "${ARTIFACTORY}", path: "${ENVIRONEMENT}/${FUNCTION}/${FUNCTION}.zip")
          }
          //executePipeline();
        }
      }
    }
    stage('Deploy') {
      steps {
        unstash 'venv'
        //Read AWS SSM parameter store parameters 
        withAWSParameterStore(credentialsId: 'BlazePulsePipelineCredentials', naming: 'relative', path: "/${ENVIRONEMENT}", recursive: true, regionName: "${AWS_REGION}") {
          echo "PORTALADMIN_URL- ${PORTALADMIN_URL}"
          echo "INFRASERVICE_URL- ${INFRASERVICE_URL}"
          //executePipeline();
        }
      }
    }
  }
}

def executePipeline() {
  print('Executing pipeline for {} env'.format(BASE_URL))
}