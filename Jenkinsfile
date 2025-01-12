pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'script scripts/build.sh'
            }
        }
        stage('Test') {
            steps {
                sh 'script scripts/test.sh'
            }
        }
        stage('Docker Build and Push') {
            steps {
                script {
                    def app = docker.build("my-custom-image")
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_creds_id') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }
}

