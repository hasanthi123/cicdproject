pipeline {
    agent any

    environment {
        REGISTRY = "localhost:5000"
        IMAGE_NAME = "flask-cicd-demo"
        FULL_IMAGE_NAME = "localhost:5000/flask-cicd-demo"
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

        stage("Tag Image for Local Registry") {
            steps {
                script {
                    sh "docker tag $IMAGE_NAME $FULL_IMAGE_NAME"
                }
            }
        }

        stage("Push to Local Registry") {
            steps {
                script {
                    sh "docker push $FULL_IMAGE_NAME"
                }
            }
        }

        stage("Run Container") {
            steps {
                script {
                    sh "docker run -d -p 5000:5000 $FULL_IMAGE_NAME"
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

