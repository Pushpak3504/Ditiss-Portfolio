pipeline {
    agent any

    environment {
        AWS_REGION           = 'ap-south-1'
        S3_BUCKET            = 'portfolio-pushpak3504'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Pushpak3504/Ditiss-Portfolio.git'
            }
        }

        stage('Check AWS CLI') {
            steps {
                sh '''
                if command -v aws &> /dev/null
                then
                    echo "AWS CLI found."
                else
                    echo "AWS CLI not found. Please install manually on Jenkins server."
                    exit 1
                fi
                '''
            }
        }

        stage('Deploy to S3') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-jenkins']]) {
                sh '''
                export PATH=$PATH:/usr/local/bin:/usr/bin:/var/lib/jenkins/.local/bin
                echo "🚀 Deploying to S3 bucket: $S3_BUCKET"
                aws s3 sync . s3://$S3_BUCKET --region $AWS_REGION --delete --exclude ".git/*" --exclude "Jenkinsfile"
                '''
            }
        }
    }

    }

    post {
        success {
            echo 'Deployment successful'
        }
        failure {
            echo 'Deployment failed. Check the logs.'
        }
    }
}
