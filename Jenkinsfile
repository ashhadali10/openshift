pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo "Building app..."'
                sh 'oc start-build myapp'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
                sh 'curl -s http://myapp-deployment:8080 | grep "Hello"'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Check if deployment exists
                    def dcExists = sh(script: 'oc get dc/myapp-deployment || true', returnStatus: true) == 0
                    
                    if (!dcExists) {
                        sh '''
                            echo "Creating new deployment..."
                            oc new-app myapp:latest --name=myapp-deployment
                            oc expose svc/myapp-deployment
                        '''
                    } else {
                        sh 'oc rollout latest dc/myapp-deployment'
                    }
                }
            }
        }
    }
}
