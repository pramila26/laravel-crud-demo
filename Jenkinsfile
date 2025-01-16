pipeline {
    agent any

    environment {
        // Define environment variables (like server credentials, etc.)
        DEPLOY_SERVER = 'ubuntu@13.203.155.254'  // Change this to your EC2 user and IP address
        DEPLOY_PATH = '/var/www/laravel-crud-demo'  // Path on the EC2 instance
        GIT_BRANCH = 'main'  // Git branch to deploy
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the GitHub repository
                git branch: "${GIT_BRANCH}", url: 'https://github.com/pramila26/laravel-crud-demo.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install Composer dependencies
                sh 'composer install --no-dev --optimize-autoloader'
            }
        }

        stage('Deploy to Server') {
            steps {
                // Deploy the code to the EC2 instance
                sshagent(credentials: ['ec2-deploy-key']) { // Use Jenkins credentials for SSH access
                    sh """
                        # SSH into the EC2 instance and deploy
                        ssh ${DEPLOY_SERVER} << EOF
                            # Navigate to the project directory on the EC2 instance
                            cd ${DEPLOY_PATH}
                            git pull origin ${GIT_BRANCH}
                            composer install --no-dev --optimize-autoloader
                            php artisan migrate --force  # Run database migrations
                            chmod -R 775 storage bootstrap/cache  # Set the right permissions
                            php artisan config:cache
                            php artisan route:cache
                            php artisan view:cache
                            
                            # Restart Apache (ensure Apache is installed and running)
                            sudo systemctl restart apache2  # For Apache
                        EOF
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
            echo 'Deployment failed!'
        }
    }
}
