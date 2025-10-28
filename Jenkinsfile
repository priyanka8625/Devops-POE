pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "myapp:${BUILD_NUMBER}"
        CONTAINER_NAME = "myapp-container"
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/priyanka8625/Devops-POE.git'
            }
        }
        
        // REMOVED Build Application stage - Docker will handle it
        
        // REMOVED Run Tests stage - Docker will handle it
        
        stage('Build Docker Image') {
            steps {
                script {
                    bat "docker build -t %DOCKER_IMAGE% ."
                }
            }
        }
        
        stage('Stop Old Container') {
            steps {
                script {
                    bat '''
                        docker stop %CONTAINER_NAME% 2>nul || exit 0
                        docker rm %CONTAINER_NAME% 2>nul || exit 0
                    '''
                }
            }
        }
        
        stage('Run Docker Container') {
            steps {
                script {
                    bat "docker run -d --name %CONTAINER_NAME% -p 8080:5000 %DOCKER_IMAGE%"
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
            echo 'Application running at http://localhost:8080'
        }
        failure {
            echo 'Pipeline failed!'
        }
        always {
            script {
                bat 'docker image prune -f || exit 0'
            }
        }
    }
}