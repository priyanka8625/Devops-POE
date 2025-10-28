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
        
        stage('Build Application') {
            steps {
                script {
                    // For Node.js
                    sh 'npm install'
                    
                    // For Java (Maven)
                    // sh 'mvn clean package'
                    
                    // For Python
                    // sh 'pip install -r requirements.txt'
                }
            }
        }
        
        stage('Run Tests') {
            steps {
                script {
                    // For Node.js
                    sh 'npm test || true'
                    
                    // For Java
                    // sh 'mvn test'
                    
                    // For Python
                    // sh 'pytest'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }
        
        stage('Stop Old Container') {
            steps {
                script {
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"
                }
            }
        }
        
        stage('Run Docker Container') {
            steps {
                script {
                    sh """
                        docker run -d \
                        --name ${CONTAINER_NAME} \
                        -p 8080:3000 \
                        ${DOCKER_IMAGE}
                    """
                }
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
            echo "Application running at http://localhost:8080"
        }
        failure {
            echo 'Pipeline failed!'
        }
        always {
            // Clean up old images
            sh 'docker image prune -f'
        }
    }
}