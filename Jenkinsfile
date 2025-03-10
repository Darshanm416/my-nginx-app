pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'darshanm416/nginx-app'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
    }
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Darshanm416/my-nginx-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE:latest .'
                }
            }
        }
        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    }
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    sh 'docker push $DOCKER_IMAGE:latest'
                }
            }
        }
        stage('Deploy Container') {
            steps {
                script {
                    sh '''
                        docker stop myapp || true
                        docker rm myapp || true
                        docker run -d -p 8080:80 --name myapp $DOCKER_IMAGE:latest
                    '''
                }
            }
        }
    }
}

