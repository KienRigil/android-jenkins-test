pipeline {
  agent any
  stages {
    stage('Build Debug') {
      steps {
        sh './gradlew clean assembleDebug'
      }
    }
    stage('Archive Debug Artifact') {
      steps {
        archiveArtifacts(fingerprint: true, artifacts: 'app/build/outputs/apk/debug/*.apk')
      }
    }
    stage('Test') {
      steps {
        sh './gradlew test'
      }
    }
    stage('Save Test Report') {
      steps {
        junit 'app/build/test-results/*/*.xml'
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
    }
    stage('Archive Release Artifact') {
      steps {
        archiveArtifacts(artifacts: 'app/build/outputs/apk/release/*.apk', fingerprint: true)
      }
    }
  }
}