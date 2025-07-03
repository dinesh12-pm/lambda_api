pipeline {
    agent any

    environment {
        GIT_CREDENTIALS = credentials('dinesh12-pm')
        AWS_ACCESS_KEY_ID = credentials('LGaKn5jnbnUkKJl1WeyIyl0JguKseVMG4mTV6Xg9')
        AWS_SECRET_ACCESS_KEY = credentials('LGaKn5jnbnUkKJl1WeyIyl0JguKseVMG4mTV6Xg9')
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
