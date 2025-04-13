pipeline {
  agent any
  triggers { pollSCM('* * * * *') }  // Checks Git every minute
  stages {
    stage('Build') { steps { sh 'echo "Building..."' } }
    stage('Test') { steps { sh 'echo "Testing..."' } }
    stage('Deploy') { 
      when { expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') } }
      steps { sh 'oc rollout latest dc/myapp' }  // Deploys if build/test pass
    }
  }
}
