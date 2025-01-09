pipeline {
    agent any

    environment {
        DEPLOY_SERVER_IP = '51.20.93.133'        // Replace with your EC2 instance IP
        DEPLOY_PATH = '/var/www/laravel-crud-demo'    // Path on EC2 to deploy the app
        GIT_REPO = 'https://github.com/pramila26/laravel-crud-demo.git' // Your Git repository URL
        SSH_CREDENTIALS_ID = '13.53.39.179'   // The ID of the SSH credential you set up
    }

    stages {
        stage('Example') {
            steps {
                // Use the environment variables
                sh 'echo "Deploying to $DEPLOY_SERVER at $DEPLOY_PATH"'
            }
        }
    }
}
