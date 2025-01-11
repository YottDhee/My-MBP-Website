pipeline {
    agent any

    environment {
        EC2_PUBLIC_IP = '13.126.220.169' // Replace with your EC2 instance's public IP
        DEPLOY_PORT = '5000' // Replace with your desired port (e.g., 80 for HTTP or 8080 for testing)
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull the latest changes from the repository
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
            }
        }

        stage('Start the Web Server') {
            steps {
                script {
                    // Stop any existing process on port 9090
                    sh '''
                    if lsof -t -i:${SERVER_PORT}; then
                        kill -9 $(lsof -t -i:${SERVER_PORT})
                    fi
                    '''
                    // Start the Python HTTP server
                    sh "nohup python3 -m http.server ${SERVER_PORT} --directory . > server.log 2>&1 &" 
                }
            }
        }
    }

    post {
        success {
            echo "Deployment successful! Your webpage is live at http://${EC2_PUBLIC_IP}:${DEPLOY_PORT}/"
        }
        failure {
            echo "Deployment failed. Check the logs for details."
        }
    }
}

