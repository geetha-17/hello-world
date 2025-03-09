pipeline {
    agent any

    environment {
        IMAGE_NAME = 'geetha8500/hello-world-python'
    }

    stages {
        stage('Clone Repository') {
    steps {
        withCredentials([usernamePassword(credentialsId: 'ee8e722c-b38f-4b4c-90b9-fd32ccde08a1', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASSWORD')]) {
            sh '''
                if [ -d "hello-world-python" ]; then
                    rm -rf hello-world-python
                fi
                git clone https://$GIT_USER:$GIT_PASSWORD@github.com/geetha-17/hello-world.git
            '''
        }
    }
}


        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: '45e3064a-7b6c-4fe3-a314-15fded797ec4', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }
    }

    post {
        success {
            echo 'Build and Push Successful!'
        }
        failure {
            echo 'Build Failed!'
        }
    }
}
