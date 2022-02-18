pipeline {
   agent any

   environment {
     // You must set the following environment variables
     // ORGANIZATION_NAME
     // YOUR_DOCKERHUB_USERNAME (it doesn't matter if you don't have one)
     
     ORGANIZATION_NAME = "vny-org-cicd"
     SERVICE_NAME = "fleetman-queue"
     ECR_URI = "794077140502.dkr.ecr.us-east-2.amazonaws.com/fleetman-queue"
     REPOSITORY_TAG ="${ECR_URI}:${BUILD_ID}"
   }

   stages {
      
      stage('Build') {
         steps {
           sh 'wget -O activemq.tar.gz https://archive.apache.org/dist/activemq/5.14.3/apache-activemq-5.14.3-bin.tar.gz'
           sh 'tar -xzf activemq.tar.gz'
         }
      }

      stage('Build and Push Image') {
         steps {
           sh "aws configure set default.region us-east-2; aws configure set aws_access_key_id 'AKIA3RYVPSYLM3NRE7EP' ; aws configure set aws_secret_access_key '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'"
           sh 'aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 794077140502.dkr.ecr.us-east-2.amazonaws.com'
          
           sh 'docker image build -t ${REPOSITORY_TAG} .'
           sh 'docker push ${REPOSITORY_TAG}'
         }
      }

      stage('Deploy to Cluster') {
          steps {
            sh 'envsubst < ${WORKSPACE}/deploy.yaml | kubectl apply -f -'
          }
      }
   }
}
