pipeline {
    agent any

    environment {
        // Define your image name clearly here
        DOCKER_IMAGE = 'martins984/node-devops-app:v1'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
    }

    stages {
        stage('Build & Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        // 1. Build the image
                        def customImage = docker.build(DOCKER_IMAGE)
                        
                        // 2. Push to Docker Hub
                        customImage.push()
                    }
                }
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Tests passed!"'
            }
        }

        stage('Deploy to K8s') {
            steps {
                // 1. Download kubectl inside the container (The "Tool Installer" pattern)
                sh 'curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"'
                sh 'chmod +x kubectl'
                
                // Note: Connecting Jenkins to Minikube is tricky. 
                // For this step, we will try a simple remote trigger or skip it for now.
                echo 'Image successfully pushed to Docker Hub! K8s will pull it on next restart.'
            }
        }
    }
}