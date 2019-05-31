#!/usr/bin/env groovy
pipeline {
  agent any
  options { skipDefaultCheckout()
            disableConcurrentBuilds()
          }

  stages {
    stage('Build Dependencies') {
      steps {
        checkout scm
        sh "npm install"
      }
    }
     stage('Package') {
        steps {
         sh  "npm run build-aws-resource"
            //send to s3 zip file
        }
    }
     stage('Build') {
         steps {
             'aws cloudformation create-stack --template-url https://anthony-bucket-one.s3-ap-northeast-1.amazonaws.com/cloudformation.template --stack-name anthonyteststack --capabilities CAPABILITY_IAM --region ap-northeast-1\n'
             'aws cloudformation wait stack-create-complete --stack-name anthonyteststack --region ap-northeast-1'
         }
     }
    }
}