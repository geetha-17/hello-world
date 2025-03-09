pipeline {
    agent any

    environment {
        IMAGE_NAME = 'geetha-17/hello-world-python'
        DOCKER_CREDENTIALS_ID = '45e3064a-7b6c-4fe3-a314-15fded797ec4'  // Make sure this ID matches your Jenkins credentials
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    // Remove existing directory to prevent conflicts
                    sh 'rm -rf hello-world-python || true'
                    sh 'git clone https://geetha-17:${GIT_PASSWORD}@github.com/geetha-17/hello-world-python.git'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME:latest hello-world-python'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: '45e3064a-7b6c-4fe3-a314-15fded797ec4', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh 'docker push $IMAGE_NAME:latest'
                }
            }
        }
    }

    post {
        failure {
            echo 'Build Failed!'
        }
        success {
            echo 'Build and Push Successful!'
        }
    }
}
