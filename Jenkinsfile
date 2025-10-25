pipeline {
    agent any

    environment {
        AWS_REGION = 'ap-south-1'
        S3_BUCKET  = 'portfolio-pushpak3504'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Pushpak3504/Ditiss-Portfolio.git'
            }
        }

        stage('Deploy to S3') {
            steps {
                withCredentials([
                    string(credentialsId: 'aws-access-key', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-key', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    sh '''
                    echo "🚀 Deploying to S3..."
                    aws s3 sync . s3://$S3_BUCKET \
                        --region $AWS_REGION \
                        --delete \
                        --exclude ".git/*" \
                        --exclude "Jenkinsfile"
                    echo "✅ Deployment complete!"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Deployment failed. Check logs.'
        }
    }
}
