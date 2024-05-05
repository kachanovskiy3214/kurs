pipeline {
    agent any
    environment {
        // Define environment variables
        DOCKERFILE_PATH = "Dockerfile" // Path to Dockerfile in GitHub repository
        DOCKER_IMAGE_NAME='nobodynow/newdocker'
    }
    stages {
        stage('Clone repository') {
            steps {
                // Clone GitHub repository
                git branch: 'master', url: 'https://github.com/kachanovskiy3214/kurs.git'
            }
        }
        stage('Build Docker image') {
            steps {
                // Build Docker image
                app = docker.build "${DOCKER_IMAGE_NAME}"
            }
        }
        stage('Push Docker image') {
            steps {
                // Push Docker image to repository
                withDockerRegistry([ credentialsId: "docker_credentials", url: "https://index.docker.io/v1/" ]) {
                    app.push()
                }
            }
        }
        stage('Run Docker image') {
            steps {
                // Run Docker image{
                docker.image("${DOCKER_IMAGE_NAME}").run('-d -p 81:80')
            }
        }
    }
}
