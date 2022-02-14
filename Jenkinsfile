pipeline {
   agent any

   environment {
     // You must set the following environment variables
     // ORGANIZATION_NAME
     // YOUR_DOCKERHUB_USERNAME (it doesn't matter if you don't have one)
     YOUR_DOCKERHUB_USERNAME = "hvny"
     ORGANIZATION_NAME = "vny-ci-cd-demo"
     SERVICE_NAME = "fleetman-queue"
     REPOSITORY_TAG="${YOUR_DOCKERHUB_USERNAME}/${ORGANIZATION_NAME}-${SERVICE_NAME}:${BUILD_ID}"
   }

   stages {
      
      stage('Build') {
         steps {
            sh '''echo No build required for Queue'''
         }
      }

      stage('Build and Push Image') {
         steps {
           sh 'docker login -u "hvny" -p "7702305501@Dhub" docker.io'
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
