pipeline {
   agent any

   environment {
       DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
       IMAGE_NAME = "81429444/pythonapp"
   }

   stages {
       stage('Build Docker Image') {
           steps {
               sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
           }
       }

       stage('Login to DockerHub') {
           steps {
               sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
           }
       }

       stage('Push Docker Image') {
           steps {
               sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'
           }
       }
   }

   post {
       always {
           sh 'docker logout'
       }

       success {
           slackSend message: ":white_check_mark: Build and Push Success: *${env.JOB_NAME}* #${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
       }

       failure {
           slackSend message: ":x: Build Failed: *${env.JOB_NAME}* #${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
       }
   }
}
