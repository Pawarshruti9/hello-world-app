pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "your-dockerhub-username/hello-world-app"
        DOCKER_CONTAINER = "hello-world-container"
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone the Git repository
                git branch: 'main', url: 'https://github.com/your-repo/hello-world-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Stop Existing Container') {
            steps {
                script {
                    // Stop and remove the existing container if it exists
                    def running = sh(script: "docker ps -q -f name=${DOCKER_CONTAINER}", returnStdout: true).trim()
                    if (running) {
                        sh "docker stop ${DOCKER_CONTAINER}"
                        sh "docker rm ${DOCKER_CONTAINER}"
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run a new container with the latest image
                    sh "docker run -d --name ${DOCKER_CONTAINER} -p 3000:3000 ${DOCKER_IMAGE}:latest"
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up dangling Docker images...'
            sh 'docker image prune -f'
        }
    }
}
