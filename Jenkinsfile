pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'simple-flask-app'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the app from GitHub
                git 'https://github.com/yourusername/simple-flask-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container
                    sh 'docker run -d -p 5000:5000 --name flask-container $DOCKER_IMAGE'
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker container and image after the job
            sh 'docker rm -f flask-container || true'
            sh 'docker rmi -f $DOCKER_IMAGE || true'
        }
    }
}
