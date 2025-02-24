@Library('Shared@main') _
pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                script {
                    hello()
                }
            }
        }
        
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
                sh 'whoami'  // Ensure correct spacing around the command
                sh 'docker build -t my-django-note-app:latest .'  // Corrected spacing
                sh 'docker images'  // Corrected spacing
                echo 'Build code successfully'  // Using echo for success message
            }
        }
        
        stage('Push Image from docker'){
            steps{
                echo 'Hello, this is push image from Docker Hub'
                withCredentials([usernamePassword(
                    credentialsId:"Jenkins-app-note-django",
                    passwordVariable:"dockerHubPass",
                    usernameVariable:"dockerHubUser")]){
                sh 'docker push ${env.dockerHubUser}/my-django-note-app:latest'}
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
