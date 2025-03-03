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
                build("ganeshmestry21","my-django-note-app","my-django-note-app" )
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
