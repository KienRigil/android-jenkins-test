pipeline {
  agent any
  stages {
    stage('Build Debug') {
      steps {
        sh './gradlew clean assembleDebug'
      }
      post {
        success {
          archiveArtifacts(fingerprint: true, artifacts: 'app/build/outputs/apk/debug/*.apk')
        }
      }
    }
    stage('Test') {
      steps {
        sh './gradlew test'
      }
      post {
        always {
          junit 'app/build/test-results/*/*.xml'
        }
      }
    }
    stage('Sanity Check') {
      steps {
        input 'Move on the release?'
      }
    }
    stage('Build Release') {
      steps {
        sh './gradlew clean assembleRelease'
      }
      post {
        success {
          archiveArtifacts(artifacts: 'app/build/outputs/apk/release/*.apk', fingerprint: true)
        }
      }
    }
  }
}