pipeline {
    agent any
    environment {
        // Define your Docker Hub credentials and image name here
        DOCKER_IMAGE = 'anurpriyanto/bologin:latest' // Image name
        KUBE_CONTEXT = 'your-kube-context'  // Kube context if you have multiple clusters
        KUBERNETES_NAMESPACE = 'default'  // Replace with your namespace
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout your repository
                git 'https://github.com/anurpriyanto/bo-login.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh '''
                        docker build -t $DOCKER_IMAGE .
                    '''
                }
            }
        }
        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhubcredentials', passwordVariable: 'Qwerty117532', usernameVariable: 'anurpriyanto')]) {
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                sh 'docker push $DOCKER_IMAGE'
                }
            }
        }
        
    }
    
}
