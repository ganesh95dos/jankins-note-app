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
            steps{
                dockerpush("dockerHubCreds","notes-app","latest")
            }
        }

        stage('Deploy Code') {
            steps{
                deploy()
            }
        }
    }
}
