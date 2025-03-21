pipeline {
    agent any

    environment {
        REGISTRY = 'docker.io'  // Docker Hub Registry
        DOCKERHUB_USERNAME = 'hasanthi123'  // Docker Hub username
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
                    sh 'docker build -t ${IMAGE_NAME} .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(credentialsId: 'docker-hub-credentials', 
                                     usernameVariable: 'DOCKER_USERNAME', 
                                     passwordVariable: 'DOCKER_PASSWORD'),
                    string(credentialsId: 'docker-hub-pat', variable: 'DOCKER_PAT')
                ]) {
                    sh 'echo "$DOCKER_PAT" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh 'docker push ${IMAGE_NAME}'
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh 'docker ps -a'
                    sh 'docker stop flask-cicd-demo || true && docker rm flask-cicd-demo || true'
                    sh 'docker run -d -p 5006:5000 --name flask-cicd-demo ${IMAGE_NAME}'
                }
            }
        }

        stage('Post Actions') {
            steps {
                script {
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
