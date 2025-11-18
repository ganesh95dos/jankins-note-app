@Library('Shared@main') _
pipeline {
    agent any

    stages {            
        stage("Clone Code"){
            steps{
                clone("https://github.com/ganesh95dos/jankins-note-app.git","dev")
            }
        }

        stage("Build and Test"){
            steps{
                docker_build("my-django-note-app","latest")
            }
        }
        
        stage('Build and Push Image to Docker Hub') {
            steps {
                docker_push("my-django-note-app","latest","ganeshmestry21", )
                echo 'Hello, this image has been pushed to Docker Hub successfully'
            }
        }

        stage('Deploy Code') {
            steps{
                
                deploy()
            }
        }
    }
}
