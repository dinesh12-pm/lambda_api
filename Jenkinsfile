pipeline {
    agent any

    environment {
        STAGE = "dev"
    }

    stages {
        stage('Checkout Code') {
            steps {
                cleanWs()
                git(
                    url: 'https://github.com/dinesh12-pm/lambda_api.git',
                    credentialsId: 'your-git-creds-id',
                    branch: 'main'
                )
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
                sh 'npm install serverless@3'  // ✅ Local install
            }
        }

        stage('Deploy to AWS Lambda') {
            steps {
                withCredentials([
                    string(credentialsId: 'aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    sh 'npx serverless deploy --stage $STAGE --verbose'  // ✅ Uses locally installed Serverless
                }
            }
        }
    }

    post {
        success {
            echo '✅ Lambda Deployed Successfully!'
        }
        failure {
            echo '❌ Deployment Failed.'
        }
    }
}
