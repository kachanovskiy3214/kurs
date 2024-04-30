pipeline {
    agent any

    environment {
        // Define environment variables
        DOCKERFILE_PATH = "Dockerfile" // Path to Dockerfile in GitHub repository
        DOCKER_IMAGE_NAME='newdocker'
        DOCKER_TAG = "final" // Docker image tag
        
    }

    stages {
        stage('Restart Docker') {
            steps {
                // Restart Docker server
                sh 'sudo systemctl restart docker'
            }
        }
        stage('Clone repository') {
            steps {
                // Clone GitHub repository
                git branch: 'master', url: 'https://github.com/kachanovskiy3214/kurs.git'
            }
        }

        stage('Build Docker image') {
            steps {
                // Build Docker image
                script {
                   docker.build "${DOCKER_IMAGE_NAME}:${DOCKER_TAG}"
                }
            }
        }

        stage('Save artifacts') {
            steps {
                // Save Dockerfile as an artifact
                archiveArtifacts artifacts: 'Dockerfile', onlyIfSuccessful: false
            }
        }

        stage('Push Docker image') {
            steps {
                
                // Push Docker image to repository
                   withDockerRegistry([ credentialsId: "docker-hub-credentials", url: "" ]) {
                    bat "docker push devopsglobalmedia/teamcitydocker:build"
        }
    }
}
        stage('Run Docker image') {
            steps {
                // Run Docker image
                script {
                    docker.image("${DOCKER_IMAGE_NAME}:${DOCKER_TAG}").run('-d -p 81:80')
                }
            }
        }
    }
}
