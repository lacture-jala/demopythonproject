pipeline{
    agent any

    environment{
        AWS_ACCOUNT_ID = "156172784305"
        AWS_REGION= "ap-south-1"
        ECR_REPO_NAME="ECR_REPO_NAME"
        IMAGE_TAG="latest"
    }
    stages{
        stage('checkout'){
            steps{
                script{
                    checkout scm
                }
            }
        }
         stage('Retrieve an authentication token and authenticate your Docker client to your registry. Use the AWS CLI:'){
            steps{
                sh '''
                aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                '''
            }
        }
        stage('Build Docker image'){
            steps{
                sh '''
                docker build -t ${ECR_REPO_NAME} .
                '''
            }
        }
        stage('Taggin image'){
            steps{
                sh '''
                docker tag ${ECR_REPO_NAME}:${IMAGE_TAG} ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}
                '''
            }
        }
        stage('Pushing Docker Image to ECR'){
            steps{
                sh '''
                docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${IMAGE_TAG}
                '''
            }
        }
    }
}