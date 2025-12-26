pipeline {
    agent any

    environment {
        // 1. We separate the Repo Name from the Tag
        DOCKER_REPO = 'martins984/node-devops-app'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
        
        // 2. Create a dynamic tag using the Jenkins BUILD_NUMBER
        // Result will look like: v1.0.23, v1.0.24, etc.
        IMAGE_TAG = "v1.0.${BUILD_NUMBER}"
    }

    stages {
        stage('Build & Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        // 3. Build with the specific version tag
                        def customImage = docker.build("${DOCKER_REPO}:${IMAGE_TAG}")
                        
                        // 4. Push the versioned tag (e.g., v1.0.24)
                        customImage.push()
                        
                        // 5. Also push 'latest' so people can easily pull the newest one
                        customImage.push('latest')
                    }
                }
            }
        }

        stage('Test') {
            steps {
                echo 'Running Unit Tests inside the container...'
                script {
                   // Test the specific version we just built
                   sh "docker run --rm ${DOCKER_REPO}:${IMAGE_TAG} npm test" 
                }
            }
        }

        stage('Deploy to K8s') {
            steps {
                // 6. Download kubectl (Standard Tool Installer pattern)
                sh 'curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"'
                sh 'chmod +x kubectl'
                
                // 7. THE MAGIC TRICK: explicitly set the image to the new version
                // This forces K8s to perform a Rolling Update because the image name changed.
                sh "kubectl set image deployment/node-app-deployment node-app=${DOCKER_REPO}:${IMAGE_TAG}"
                
                // Optional: Verify the rollout status
                sh "kubectl rollout status deployment/node-app-deployment"
            }
        }
    }
}