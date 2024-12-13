pipeline {
    agent any

    environment {
        IMAGE_NAME = "anurpriyanto/bo-login"
        IMAGE_TAG = "latest"
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
                    bat "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
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
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                        bat """
                        echo ${DOCKERHUB_PASSWORD} | docker login -u ${DOCKERHUB_USERNAME} --password-stdin
                        docker push ${IMAGE_NAME}:${IMAGE_TAG}
                        """
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            script {
                bat 'docker rmi ${IMAGE_NAME}:${IMAGE_TAG} || echo Cleanup skipped'
            }
        }
    }
}
