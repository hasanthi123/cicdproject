pipeline {
    agent any
    
    environment {
        REGISTRY = 'localhost:5005'
        IMAGE_NAME = 'flask-cicd-demo'
        REGISTRY_URL = "${REGISTRY}/${IMAGE_NAME}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the Git repository
                git 'https://github.com/hasanthi123/cicdproject.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t ${REGISTRY_URL}:latest .'
                }
            }
        }

        stage('Push to Local Registry') {
            steps {
                script {
                    // Push the Docker image to the local registry
                    sh 'docker push ${REGISTRY_URL}:latest'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container with the image in the registry
                    sh 'docker run -d -p 5005:5000 --name flask-cicd-demo ${REGISTRY_URL}:latest'
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker containers after the pipeline is done
            sh 'docker ps -a'
        }
    }
}
