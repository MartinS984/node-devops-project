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

        stage('Deploy') {
            steps {
                sh 'docker stop node-app || true' // stop command
                sh 'docker rm node-app || true' // remove command
                sh 'docker run -d -p 3000:3000 --name node-app node-app:latest' // run command
            }
        }
    }
}