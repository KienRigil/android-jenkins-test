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
        junit 'app/build/test-results/*.xml'
      }
    }
  }
  post {
    success {
      mail(to: 'kien@rigil.com', subject: "Succeeded Pipeline: ${currentBuild.fullDisplayName}", body: "Check out ${env.BUILD_URL}")

    }

    failure {
      mail(to: 'kien@rigil.com', subject: "Failed Pipeline: ${currentBuild.fullDisplayName}", body: "Something is wrong with ${env.BUILD_URL}")

    }

  }
}