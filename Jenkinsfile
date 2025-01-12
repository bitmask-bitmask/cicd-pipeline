pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "kateprogrammistka/test-image" // Replace with your image name
        REGISTRY_CREDENTIALS = 'docker_hub_creds_id' // Jenkins credentials ID for Docker Hub
    }

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${DOCKER_IMAGE}:${env.BUILD_NUMBER} .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', REGISTRY_CREDENTIALS) {
                        def app = docker.image("${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }
}
