pipeline {
    agent none  // No default agent; specify per stage
    stages {
        stage('Clone Repository') {
            agent { label 'docker' }  // Can run on any agent (Jenkins master is fine)
            steps {
                script {
                    sh 'rm -rf hello-world'
                    sh 'git clone https://github.com/geetha-17/hello-world.git'
                }
            }
        }
        stage('Build Docker Image') {
            agent { label 'docker' }  // Run on the Docker agent
            steps {
                script {
                    sh 'docker build -t 54.162.129.163/myproject/hello-world:latest hello-world'
                }
            }
        }
        stage('Login to Harbor Registry') {
            agent { label 'docker' }  // Run on the Docker agent
            steps {
                // Add your Harbor login commands here, e.g.:
                sh 'docker login 54.162.129.163 -u admin -p Harbor12345'
            }
        }
        stage('Push Docker Image') {
            agent { label 'docker' }  // Run on the Docker agent
            steps {
                sh 'docker push 54.162.129.163/myproject/hello-world:latest'
            }
        }
    }
    post {
        failure {
            echo 'Build Failed!'
        }
    }
}
