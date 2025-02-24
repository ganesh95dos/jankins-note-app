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
        
        stage('Copy Code') {
            steps {
                sh "whoami"
            clone("https://github.com/ganesh95dos/jankins-note-app.git","dev")
            }
        }

        stage('Build Code') {
            steps{
            dockerbuild("notes-app","latest")
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
            steps {
                echo 'Hello, deploying the code using Docker Compose'
                // Run docker-compose to start containers in detached mode
                sh 'docker-compose down'  // Stop and remove any existing containers
                sh 'docker-compose up -d' // Start the containers in detached mode

                echo 'Deployed code successfully'  // Success message
            }
        }
    }
}
