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
            steps {
                echo 'Hello, this is building the code'
                //sh 'docker rmi -f $(docker images -q) || true'  // This removes all old images

                // Build the Docker image
                sh 'docker build -t my-django-note-app:latest .'

                // List images after building
                sh 'docker images'
                
                // Remove untagged (dangling) images
                sh 'docker image prune -f'

                echo 'Build code successfully'  // Echo for success message

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
