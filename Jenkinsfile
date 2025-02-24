pipeline {
    agent any

    stages {
        stage('Copy Code') {
            steps {
                echo 'Hello, this is copying the code'
                git branch: 'dev', url: 'https://github.com/ganesh95dos/jankins-note-app.git'
                echo 'Code copied successfully'
            }
        }

        stage('Build Code') {
            steps {
                echo 'Hello, this is building the code'
                // Remove old images to avoid conflicts with new build
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
