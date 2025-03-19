pipeline {
    agent any

    environment {
        REGISTRY = 'localhost:5005'  // Local Docker Registry URL
        IMAGE_NAME = 'flask-cicd-demo'
        REGISTRY_URL = "${REGISTRY}/${IMAGE_NAME}"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git 'https://github.com/hasanthi123/cicdproject.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${REGISTRY_URL}:latest .'
                }
            }
        }

        stage('Push to Local Registry') {
            steps {
                script {
                    sh 'docker push ${REGISTRY_URL}:latest'
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // Run container from the local registry on port 5005
                    sh 'docker run -d -p 5005:5000 --name flask-cicd-demo ${REGISTRY_URL}:latest'
                }
            }
        }

        stage('Post Actions') {
            steps {
                script {
                    // List running containers
                    sh 'docker ps -a'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
