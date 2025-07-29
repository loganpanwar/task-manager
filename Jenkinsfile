pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "loganpanwar/task-manager"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-creds', url: '']) {
                    script {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh 'docker rm -f task-manager || true'
                    sh 'docker run -d -p 5000:5000 --name task-manager loganpanwar/task-manager'
                }
            }
        }
    }
}

