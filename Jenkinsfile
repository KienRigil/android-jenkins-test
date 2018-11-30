pipeline {
  agent any
  stages {
    stage('Debug - Build') {
      steps {
        sh './gradlew clean assembleDebug'
      }
      post {
        success {
          archiveArtifacts(fingerprint: true, artifacts: 'app/build/outputs/apk/debug/*.apk')
        }
      }
    }
    stage('Debug - Unit Test') {
      steps {
        sh './gradlew testDebugUnitTest'
      }
      post {
        always {
          junit 'app/build/test-results/*/*.xml'
        }
      }
    }
  }
}