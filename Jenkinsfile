pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo 'Building Docker Image...'
                // This runs the same command you ran manually!
                sh 'docker build -t node-app:latest .' 
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running Tests...'
                // Placeholder for real tests
                sh 'echo "Tests passed!"' 
            }
        }

        stage('Deploy to K8s') {
    steps {
        // 1. Pass the new image from Docker to Minikube
        sh 'minikube image load node-app:latest'
        
        // 2. Force Kubernetes to restart the pods (picking up the new image)
        sh 'kubectl rollout restart deployment/node-app-deployment'
        
        // 3. Verify it worked
        sh 'kubectl rollout status deployment/node-app-deployment'
    }
}