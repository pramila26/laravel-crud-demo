pipeline {
    agent any

    environment {
        DEPLOY_SERVER_IP = '15.207.54.233'
        DEPLOY_PATH = '/var/www/laravel-app'
        GIT_REPO = 'https://github.com/pramila26/laravel-crud-demo.git'  // Your Git repository URL
        SSH_CREDENTIALS_ID = 'ec2-deploy-key'    // Your Jenkins SSH credentials ID
    }

    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: "${SSH_CREDENTIALS_ID}", url: "${GIT_REPO}"
            }
        }

        stage('Deploy') {
            steps {
                sshagent(credentials: ["${SSH_CREDENTIALS_ID}"]) {
                    sh """
                    cd ${DEPLOY_PATH}
                    composer install --no-dev --optimize-autoloader
                    cp .env.example .env
                    php artisan key:generate
                    php artisan migrate --force
                    php artisan config:cache
                    php artisan route:cache
                    php artisan view:cache
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
