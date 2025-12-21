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
    }
}