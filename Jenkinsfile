@Library('Shared@main') _
pipeline {
    agent any

    stages {        
        stage('hello') {
            steps {
                script {
                    // Call the hello() method from the shared library
                    echo hello()  // This should print "Hello Dosto"
                }
            }
        }     
        
        stage("Clone Code"){
            steps{
                clone ("https://github.com/ganesh95dos/jankins-note-app.git","dev"
            }
        }

        stage("Build and Test"){
            steps{
                sh "docker build . -t my-django-note-app"
            }
        }
        
        stage('Push Image to Docker Hub') {
            steps {
                echo 'Hello, this is pushing the image to Docker Hub'
                withCredentials([usernamePassword(
                    credentialsId: "Jenkins-app-note-django", 
                    passwordVariable: "dockerHubPass", 
                    usernameVariable: "dockerHubUser"
                )]) {
                    // Tagging the image
                    sh 'docker tag my-django-note-app:latest ${dockerHubUser}/my-django-note-app:latest'

                    // Pushing the image to Docker Hub
                    sh "docker push ${dockerHubUser}/my-django-note-app:latest"
                }
                echo 'Hello, this image has been pushed to Docker Hub successfully'
            }
        }

        stage('Deploy Code') {
            steps{
                sh "docker-compose down"
                sh "docker-compose up -d"
            }
        }
    }
}
