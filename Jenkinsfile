pipeline {
    agent any

    environment {
        IMAGE_NAME = "your-dockerhub-username/cicd-demo"
    }

    stages {
        stage("Clone Repository") {
            steps {
                git "https://github.com/hasanthi123/cicdproject.git"
            }
        }

        stage("Build Docker Image") {
            steps {
                script {
                    sh "docker build -t $IMAGE_NAME ."
                }
            }
        }

        stage("Run Container") {
            steps {
                script {
                    sh "docker run -d -p 5000:5000 $IMAGE_NAME"
                }
            }
        }

        stage("Push to Docker Hub") {
            steps {
                script {
                    sh "docker login -u your-dockerhub-username -p your-dockerhub-password"
                    sh "docker push $IMAGE_NAME"
                }
            }
        }
    }

    post {
        always {
            sh "docker ps -a"
        }
    }
}
