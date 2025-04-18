pipeline {
    agent any

    // Configure triggers (optional, for automated builds)
    triggers {
        pollSCM('* * * * *') // Polls SCM every minute for changes (optional)
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building app...'
                sh 'oc start-build myapp'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'curl -s http://myapp-deployment:8080 | grep "Hello"'
            }
        }

        stage('Deploy') {
            when {
                expression { return currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                script {
                    // Check if deployment exists
                    def deploymentExists = sh(script: 'oc get deployment/myapp-deployment || true', returnStatus: true) == 0
                    
                    if (!deploymentExists) {
                        echo "Deployment does not exist. Creating new deployment..."
                        sh '''
                            oc new-app myapp:latest --name=myapp-deployment
                            oc expose svc/myapp-deployment
                        '''
                    } else {
                        echo "Deployment exists. Restarting deployment..."
                        sh 'oc rollout restart deployment/myapp-deployment'
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs for details.'
        }
    }
}
