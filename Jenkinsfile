pipeline {
    agent any

    environment {
        GIT_CREDENTIALS = credentials('your-git-creds-id')
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        STAGE = "dev"
    }

    stages {
        stage('Checkout Code') {
            steps {
                cleanWs()
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://your-git-repo-url.com/lambda-api.git',
                        credentialsId: "${GIT_CREDENTIALS}"
                    ]]
                ])
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Deploy to AWS Lambda') {
            steps {
                sh 'npx serverless deploy --stage ${STAGE} --verbose'
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
