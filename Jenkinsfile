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
         sh "aws s3 ls"
         sh "aws s3 cp amazonexample.zip s3://anthony-bucket-one/amazonexample.zip"
         sh "aws s3 cp cloudformation.template s3://anthony-bucket-one/cloudformation.template"
        }
    }
     stage('Build') {
         steps {
          sh 'aws cloudformation create-stack --template-url https://anthony-bucket-one.s3-eu-west-1.amazonaws.com/cloudformation.template --stack-name anthonyteststack --capabilities CAPABILITY_IAM --region ap-northeast-1\n'
          sh 'aws cloudformation wait stack-create-complete --stack-name anthonyteststack --region ap-northeast-1'
         }
     }
    }
}