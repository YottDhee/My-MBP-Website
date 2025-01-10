pipeline {
    agent any

    environment {
        // Define paths for easier reference
        WEB_SERVER_PATH = '/var/www/html'
        REPO_DIR = 'My-MBP-Website' // Adjust based on your repository
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository and pull latest changes
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install Apache or any necessary dependencies
                    echo "Installing necessary dependencies..."
                    sh """
                        sudo yum install -y httpd
                    """
                }
            }
        }

        stage('Deploy Web Files') {
            steps {
                script {
                    // Copy your web files to the server's web directory
                    echo "Deploying website files to web server directory..."
                    sh """
                        sudo cp -r ${WORKSPACE}/* ${WEB_SERVER_PATH}/
                    """
                }
            }
        }

        stage('Start/Restart Web Server') {
            steps {
                script {
                    // Restart Apache web server to reflect the changes
                    echo "Restarting Apache web server..."
                    sh """
                        sudo systemctl restart httpd
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Webpage deployment successful! Visit your site to check."
        }
        failure {
            echo "Webpage deployment failed. Please check the logs for errors."
        }
    }
}

