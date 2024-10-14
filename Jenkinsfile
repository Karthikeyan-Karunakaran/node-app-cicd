pipeline {
	agent any
		stages {
			stage('Clone Repository') {
				steps {
					git 'https://github.com/yourusername/docker-node-app.git';
				}
			}
			stage('Build Docker Image') {
				steps {
					script {
						dockerImage = docker.build("docker-node-app")
					}
				}
			}
			stage('Push to Docker Hub') {
				steps {
					script {
						docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
							dockerImage.push("latest")
						}
					}
				}
			}
			stage('Deploy to EC2') {
				steps {
					sshagent(['ec2-ssh-key']) {
						sh 'ssh ec2-user@44.200.161.79; sudo docker pull karthikeyankarunakaran/docker-node-app:latest'
						sh 'ssh ec2-user@44.200.161.79; sudo docker run -d -p 3000:3000 karthikeyankarunakaran/docker-node-app:latest'
					}
				}
			}
		}
}
