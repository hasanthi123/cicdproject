pipeline {
    agent any

    environment {
        REGISTRY = 'docker.io'  
        DOCKERHUB_USERNAME = 'hasanthi123'  
        IMAGE_NAME = 'hasanthi123/flask-cicd-demo'
        REGISTRY_URL = "${REGISTRY}/${IMAGE_NAME}"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/hasanthi123/cicdproject.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME} ."  // Ensures latest tag
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'dckr_pat_TXhSJhpRyyGyGAOb_BjUeGO3iNQ', variable: 'H@santhi123')]) {
                    sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKERHUB_USERNAME" --password-stdin'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh "docker tag ${IMAGE_NAME} ${IMAGE_NAME}:v1"
                    sh "docker push ${IMAGE_NAME}:v1"

                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // List containers before stopping
                    sh 'docker ps -a'

                    // Stop and remove existing container if running
                    sh 'docker ps -q --filter "name=flask-cicd-demo" | grep -q . && docker stop flask-cicd-demo && docker rm flask-cicd-demo || true'

                    // Run the new container
                    sh "docker run -d -p 5005:5000 --name flask-cicd-demo ${IMAGE_NAME}:latest"
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
