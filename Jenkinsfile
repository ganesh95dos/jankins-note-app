pipeline {
    agent any

    stages {
        stage('Copy Code') {
            steps {
                echo 'Hello, this is copy code'
                git branch: 'dev', url: 'https://github.com/ganesh95dos/jankins-note-app.git'
                echo 'Code copied successfully'
            }
        }

        stage('Build Code') {
            steps {
                echo 'Hello, this is Build code'
                sh 'docker rmi -f $(docker images -q) || true'  // Remove all images
                
                sh 'docker build -t my-django-note-app:latest .'
                
                sh 'docker images'
                echo 'Build code successfully'  // Using echo for success message
            }
        }

        stage('Push Image from Docker') {
            steps {
                echo 'Hello, this is push image from Docker Hub'
                withCredentials([usernamePassword(
                    credentialsId: "Jenkins-app-note-django",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]) {
                // Tag the image first (optional but recommended for proper tagging)
                sh 'docker -t my-django-note-app:latest ${dockerHubUser}/my-django-note-app:latest'
                sh 'docker push ${env.dockerHubUser}/my-django-note-app:latest'
                }
                echo 'Hello, this is push image to Docker Hub successfully'
            }
        }

        stage('Deploy Code') {
            steps {
                sh 'docker-compose up -d'  // Run docker-compose to start containers
                echo 'Deployed code successfully'  // Using echo to print the success message
            }
        }
    }
}
