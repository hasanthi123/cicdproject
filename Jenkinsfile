pipeline {
    agent any

    environment {
        REGISTRY = 'docker.io'  // Docker Hub Registry
        DOCKERHUB_USERNAME = 'hasanthi123'  // Replace with your Docker Hub username
        IMAGE_NAME = "${DOCKERHUB_USERNAME}/flask-cicd-demo:v1"
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
                script {
                    withCredentials([usernamePassword(credentialsId: 'dckr_pat_TXhSJhpRyyGyGAOb_BjUeGO3iNQ', usernameVariable: 'hasanthi123', passwordVariable: 'H@santhi123')]) {
                        sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh 'docker push ${IMAGE_NAME}:latest'
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    // List containers before stopping
                    sh 'docker ps -a'

                    // Stop and remove existing container if running
                    sh 'docker stop flask-cicd-demo || true && docker rm flask-cicd-demo || true'

                    // Run the new container
                    sh 'docker run -d -p 5005:5000 --name flask-cicd-demo ${IMAGE_NAME}:latest'
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

