pipeline {
    agent any
    environment {
        SERVER_PORT = 9090  // Define the port for the server
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building the project...'
            }
        }
        stage('Serve HTML Page') {
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
        always {
            echo 'Stopping the server...'
            sh '''
            if lsof -t -i:${SERVER_PORT}; then
                kill -9 $(lsof -t -i:${SERVER_PORT})
            fi
            '''
        }
    }
}
