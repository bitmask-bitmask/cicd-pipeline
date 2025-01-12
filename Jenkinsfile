pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'http://0.0.0.0:5000'  // Replace with your private registry URL
        DOCKER_IMAGE = 'my-test-image'                  // Your image name
        REGISTRY_CREDENTIALS = 'docker_registry_creds'  // Jenkins credentials ID for your registry
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh "docker build -t ${DOCKER_IMAGE}:${env.BUILD_NUMBER} ."
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    // Tag the image for the private registry
                    sh "docker tag ${DOCKER_IMAGE}:${env.BUILD_NUMBER} ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${env.BUILD_NUMBER}"
                    sh "docker tag ${DOCKER_IMAGE}:${env.BUILD_NUMBER} ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:latest"
                }
            }
        }

        stage('Push to Private Registry') {
            steps {
                script {
                    docker.withRegistry("${DOCKER_REGISTRY}", "${REGISTRY_CREDENTIALS}") {
                        def app = docker.image("${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }
}
