pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo "Building the app..."'
                sh 'oc start-build myapp'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
                sh 'curl -s http://myapp-deployment:8080 | grep "Hello"'  # Simple test
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo "Deploying to OpenShift..."'
                sh 'oc rollout latest dc/myapp-deployment'
            }
        }
    }
}
