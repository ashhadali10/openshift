pipeline {
  agent any
  stages {
    stage('Build') { steps { sh 'echo "Building the app"' } }
    stage('Test') { steps { sh 'echo "Running tests"' } }
    stage('Deploy') { steps { sh 'echo "Deploying to OpenShift"' } }
  }
}
