pipeline {
    agent any

    environment {
        DEPLOY_SERVER_IP = '15.207.115.183'        // Replace with your EC2 instance IP
        DEPLOY_PATH = '/var/www/laravel-app'    // Path on EC2 to deploy the app
        GIT_REPO = 'https://github.com/pramila26/laravel-crud-demo.git' // Your Git repository URL
        SSH_CREDENTIALS_ID = 'ec2-deploy-key'   // The ID of the SSH credential you set up
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Checkout code from Git
                git credentialsId: "${SSH_CREDENTIALS_ID}", url: "${GIT_REPO}"
            }
        }

        stage('Transfer Files') {
            steps {
                // Transfer files to EC2 instance
                sshagent(credentials: ["${SSH_CREDENTIALS_ID}"]) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ubuntu@${DEPLOY_SERVER_IP} 'mkdir -p ${DEPLOY_PATH}'
                    rsync -avz --delete --exclude .git ./ ubuntu@${DEPLOY_SERVER_IP}:${DEPLOY_PATH}
                    """
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install Laravel dependencies on EC2 instance
                sshagent(credentials: ["${SSH_CREDENTIALS_ID}"]) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ubuntu@${DEPLOY_SERVER_IP} '
                    cd ${DEPLOY_PATH} &&
                    composer install --no-dev --optimize-autoloader &&
                    cp .env.example .env &&
                    php artisan key:generate &&
                    php artisan migrate --force &&
                    php artisan config:cache &&
                    php artisan route:cache &&
                    php artisan view:cache'
                    "
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}