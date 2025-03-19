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
                    // Build the image directly with the correct name
                    sh "docker build -t $FULL_IMAGE_NAME ."
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
                    // Run Flask app on a different port (5001)
                    sh "docker run -d -p 5001:5000 $FULL_IMAGE_NAME"
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
