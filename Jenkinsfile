pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "myapp"
        DOCKER_TAG = "latest"
        HARBOR_URL = "http://<AWS_INSTANCE_IP>"
        HARBOR_PROJECT = "library"
        HARBOR_USER = "admin"
        HARBOR_PASSWORD = "Harbor12345"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/geetha-17/hello-world.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${HARBOR_URL}/${HARBOR_PROJECT}/${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }

        stage('Login to Harbor') {
            steps {
                sh "docker login ${HARBOR_URL} -u ${HARBOR_USER} -p ${HARBOR_PASSWORD}"
            }
        }

        stage('Push Image to Harbor') {
            steps {
                sh "docker push ${HARBOR_URL}/${HARBOR_PROJECT}/${DOCKER_IMAGE}:${DOCKER_TAG}"
            }
        }
    }
}
