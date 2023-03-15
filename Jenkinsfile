pipeline {
  agent any
  
  environment {
    AWS_ACCESS_KEY_ID = credentials('aws-access-key')
    AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key')
    AWS_REGION = 'us-east-1'
    S3_BUCKET_NAME = 'my-react-app-bucket'
  }

  stages {
    stage('Build') {
      steps {
        sh 'npm install'
        sh 'npm run build'
        archiveArtifacts artifacts: 'build/**/*', allowEmptyArchive: true
      }
    }
    
    stage('Deploy to S3') {
      steps {
        withAWS(region: "${env.AWS_REGION}", credentials: 'aws-creds') {
          s3Upload(pathPattern: 'build/**/*', bucket: "${env.S3_BUCKET_NAME}")
        }
      }
    }
  }
  
  post {
    always {
      cleanWs()
    }
  }
}
