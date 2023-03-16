pipeline {
  agent any
  tools {
    nodejs '19.8.1'
  }
  
  environment {
    AWS_ACCESS_KEY_ID = credentials('deploytos3')
    AWS_SECRET_ACCESS_KEY = credentials('deploytos3')
    AWS_REGION = 'eu-west-1'
    S3_BUCKET_NAME = 'vite-project-s3-jenkins'
  }

  stages {
    stage('Build') {
      steps {
        sh 'npm install'
        sh 'npm run build'
        archiveArtifacts artifacts: 'dist/**/*', allowEmptyArchive: true
      }
    }
    
    stage('Deploy to S3') {
      steps {
        withAWS(region: "${env.AWS_REGION}", credentials: 'aws-creds') {
          s3Upload(pathPattern: 'dist/**/*', bucket: "${env.S3_BUCKET_NAME}")
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
