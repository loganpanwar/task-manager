pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'vishulogan/task-manager'  // Your Docker Hub username/repo
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/loganpanwar/task-manager.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                        echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin
                        docker push $DOCKER_IMAGE
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f task-manager || true
                    docker pull $DOCKER_IMAGE
                    docker run -d -p 5000:5000 --name task-manager $DOCKER_IMAGE
                '''
            }
        }
    }
}

