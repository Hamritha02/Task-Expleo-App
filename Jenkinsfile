pipeline {
  agent {
    label 'app'
  }
    
  stages {
    stage('Git Checkout') { 
      steps {
        checkout scm
      }
    }

    stage('Build Docker Image') {     
      parallel { 
        stage('Build Docker Image') {
          steps {
           sh 'cd app && sudo docker build -t app .'
           sh 'aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 002936919350.dkr.ecr.us-east-1.amazonaws.com'
          sh 'sudo docker tag app:latest 002936919350.dkr.ecr.us-east-1.amazonaws.com/app:${BUILD_NUMBER}'
          sh 'sudo docker push 002936919350.dkr.ecr.us-east-1.amazonaws.com/app:${BUILD_NUMBER}'      
          }
        }
      }
    }
    
    stage('Deploying App host') {
      steps {
      // sh 'sudo docker ps'
      sh 'sudo docker stop apphost || true && sudo docker rm apphost || true'
      sh 'sudo docker run -itd --name apphost -p 8080:8080 002936919350.dkr.ecr.us-east-1.amazonaws.com/app:${BUILD_NUMBER}'
      }
    }
  }
}
