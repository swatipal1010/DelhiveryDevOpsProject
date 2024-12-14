pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "swati1010/wordpress-app"
    }
    stages {
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image...'
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Push Docker Image') {
            steps {
                echo 'Pushing Docker Image to DockerHub...'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    sh 'docker push $DOCKER_IMAGE'
                }
            }
        }
        stage('Apply Kubernetes Configs') {
            steps {
                echo 'Deploying to Kubernetes...'
                sh 'kubectl apply -f mysql-deployment.yaml'
                sh 'kubectl apply -f wordpress-deployment.yaml'
            }
        }
    }
}
