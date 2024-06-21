pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('AKIAWO33UVDWHZLNAXEX')
        AWS_SECRET_ACCESS_KEY = credentials('QqIuoLlKicO/Pvk6SdhNzVUJTFzXkEVLXOjblUpY')
        AWS_DEFAULT_REGION = 'ap-south-1' // or your preferred region
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from your repository
                git url: 'https://github.com/vaidyanilesh/lambda-deploy.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install any dependencies if necessary
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Package Lambda') {
            steps {
                // Package the Lambda function code
                sh 'zip -r lambda_function.zip .'
            }
        }

        stage('Deploy Lambda') {
            steps {
                script {
                    // Deploy the Lambda function using AWS CLI
                    sh """
                    aws lambda update-function-code \
                        --function-name YourLambdaFunctionName \
                        --zip-file fileb://lambda_function.zip \
                        --region $AWS_DEFAULT_REGION
                    """
                }
            }
        }
    }
}
