pipeline {
  agent any

  tools {
    nodejs 'nodejs'
  }
  
  environment {
    // AWS_ACCESS_KEY_ID = credentials('deploytos3')
    // AWS_SECRET_ACCESS_KEY = credentials('deploytos3')
    AWS_REGION = 'eu-west-1'
    S3_BUCKET_NAME = 'vite-project-s3-jenkins'
  }

  stages {
    stage('Build') {
      steps {
        sh 'npm install'
        sh 'npm run build'
      }
    }
    
    stage('Deploy to S3') {
      steps {
        withAWS(region: "${env.AWS_REGION}", credentials: 'deploytos3') {
          s3Delete(bucket: "${env.S3_BUCKET_NAME}", path:'/')
          s3Upload(bucket: "${env.S3_BUCKET_NAME}", workingDir:'dist', includePathPattern:'**/*')
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
