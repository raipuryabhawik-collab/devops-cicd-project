pipeline {
    agent any

    environment {
        IMAGE_NAME = "devops-flask-app"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/raipuryabhawik-collab/devops-cicd-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME ./app'
            }
        }

        stage('Import Image to k3s') {
            steps {
                sh 'docker save $IMAGE_NAME:latest -o /tmp/flask.tar'
                sh 'sudo /usr/local/bin/k3s ctr images import /tmp/flask.tar'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yml'
                sh 'kubectl apply -f k8s/service.yml'
            }
        }
    }
}
