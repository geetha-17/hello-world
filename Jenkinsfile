pipeline {
    agent any

    environment {
        // Update IMAGE_NAME to point to your Harbor registry
        IMAGE_NAME = '54.162.129.163/myproject/hello-world-python'  // Replace with your Harbor URL and project
        HARBOR_CREDENTIALS_ID = 'harbor-credentials'  // Replace with your Jenkins credential ID for Harbor
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

        stage('Login to Harbor Registry') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${HARBOR_CREDENTIALS_ID}", usernameVariable: 'HARBOR_USER', passwordVariable: 'HARBOR_PASS')]) {
                    script {
                        sh 'echo $HARBOR_PASS | docker login -u $HARBOR_USER --password-stdin 54.162.129.163'  // Replace with your Harbor URL
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
            echo 'Build and Push to Harbor Successful!'
        }
    }
}
