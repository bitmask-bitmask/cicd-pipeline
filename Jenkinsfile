pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = '0.0.0.0:5000'
        DOCKER_IMAGE = 'my-test-image'                // Image name
        REGISTRY_CREDENTIALS = 'docker_hub_creds_id' // Jenkins credentials ID
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
                    docker.withRegistry("http://${DOCKER_REGISTRY}", "${REGISTRY_CREDENTIALS}") {
                        def app = docker.image("${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${env.BUILD_NUMBER}")
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }
}
