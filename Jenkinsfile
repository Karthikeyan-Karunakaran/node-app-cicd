pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Karthikeyan21001828/node-app-cicd.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image with a temporary name
                    def tempImage = docker.build("karthikeyankarunakaran/docker-node-app:latest")
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    // Push the tagged image to Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        docker.image("karthikeyankarunakaran/docker-node-app:latest").push("latest")
                    }
                }
            }
        }
        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    sh 'ssh -o StrictHostKeyChecking=no ec2-user@44.200.161.79 sudo docker pull karthikeyankarunakaran/docker-node-app:latest'
                    sh 'ssh -o StrictHostKeyChecking=no ec2-user@44.200.161.79 sudo docker run -d -p 3000:3000 karthikeyankarunakaran/docker-node-app:latest'
                }
            }
        }
    }
}
