pipeline {
    agent any

    environment {
        EC2_PUBLIC_IP = '13.126.220.169' // Replace with your EC2 instance's public IP
        DEPLOY_PORT = '5000' // Replace with your desired port (e.g., 80 for HTTP or 8080 for testing)
        WEB_DIR = '/home/ec2-user/webapp' // Path on the EC2 instance to host your webpage
    }

    stages {
        stage('Checkout') {
            steps {
                // Pull the latest changes from the repository
                checkout scm
            }
        }

        stage('Deploy to AWS EC2') {
            steps {
                script {
                    // Archive and copy files to the EC2 instance
                    sh """
                        tar -czf webapp.tar.gz *
                        scp -i /home/ec2-user/DevOps.pem webapp.tar.gz ec2-user@${EC2_PUBLIC_IP}:${WEB_DIR}
                        ssh -i /home/ec2-user/DevOps.pem ec2-user@${EC2_PUBLIC_IP} "tar -xzf ${WEB_DIR}/webapp.tar.gz -C ${WEB_DIR}"
                    """
                }
            }
        }

        stage('Start Web Server') {
            steps {
                script {
                    // Use Python's simple HTTP server for demonstration
                    sh """
                        ssh -i /home/ec2-user/DevOps.pem ec2-user@${EC2_PUBLIC_IP} "nohup python3 -m http.server ${DEPLOY_PORT} --directory ${WEB_DIR} &"
                    """
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

