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
                        echo "DeploymentConfig does not exist. Creating new deployment..."
                        sh '''
                            oc new-app myapp:latest --name=myapp-deployment
                            oc expose svc/myapp-deployment
                        '''
                    } else {
                        echo "DeploymentConfig exists. Rolling out latest version..."
                        sh 'oc rollout latest dc/myapp-deployment'
                    }
                }
            }
        }
    }
}
