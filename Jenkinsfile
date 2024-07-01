pipeline {
    agent any
    
    environment {
        AWS_REGION = 'us-east-1'
        ECR_REPO = 'myapp'
        AWS_ACCOUNT_ID = '975050149041'
        URL_REGISTRY = "${aws}.dkr.ecr.${AWS_REGION}.amazonaws.com"
    }
    
    stages { 
        stage('SCM Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/manish-io/test-jenkin.git'
            }
        }
        
        stage('Build and Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'aws', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                        // Login to ECR
                        sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${URL_REGISTRY}"

                        // Build Docker image
                        sh "docker build -t $ECR_REPO ."

                        // Tag Docker image
                        sh "docker tag $ECR_REPO:latest ${URL_REGISTRY}/$ECR_REPO:latest"

                        // Push Docker image to ECR
                        sh "docker push ${URL_REGISTRY}/$ECR_REPO:latest"
                    }
                }
            }
        }
    }
}
