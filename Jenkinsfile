pipeline {
    agent any

    environment {
        // Docker image name and tag
        IMAGE_NAME = "anurpriyanto/bo-login"
        IMAGE_TAG = "latest" // You can change this to a specific version or use BUILD_NUMBER
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning repository...'
                git branch: 'main', url: 'https://github.com/anurpriyanto/bo-login.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                script {
                    // Build the Docker image using the Dockerfile in the repo
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Push Docker Image') {
            when {
                expression { env.DOCKERHUB_USERNAME && env.DOCKERHUB_PASSWORD }
            }
            steps {
                echo 'Pushing Docker image to Docker Hub...'
                script {
                    // Log in to Docker Hub
                    sh """
                    echo "${DOCKERHUB_PASSWORD}" | docker login -u "${DOCKERHUB_USERNAME}" --password-stdin
                    docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'docker rmi ${IMAGE_NAME}:${IMAGE_TAG} || true'
        }
    }
}
