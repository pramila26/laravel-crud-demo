pipeline {
    agent any

    environment {
        DEPLOY_SERVER_IP = '16.170.141.165'        // Replace with your EC2 instance IP
        DEPLOY_PATH = '/var/www/laravel-crud-demo'    // Path on EC2 to deploy the app
        GIT_REPO = 'https://github.com/pramila26/laravel-crud-demo.git' // Your Git repository URL
        SSH_CREDENTIALS_ID = '13.61.17.158'   // The ID of the SSH credential you set up
    }

    stages {
        stage('Example') {
            steps {
                // Use the environment variables
                sh 'echo "Deploying to $DEPLOY_SERVER_IP at $DEPLOY_PATH"'
            }
        }
    }
   

    post {
        success {
            echo 'Deployment to EC2 successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
