pipeline {
  agent any
  options {
    copyArtifactPermission 'android-jenkins-multiconfiguration-test'
  }
  stages {
    stage('Debug - Build APK') {
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
          junit 'app/build/test-results/testDebugUnitTest/*.xml'
        }
      }
    }
    stage('Release - Build APK') {
      steps {
        sh './gradlew clean assembleRelease'
      }
    }
    stage('Release - Sign APK') {
      steps {
        sh '$ANDROID_HOME/build-tools/28.0.3/zipalign -v -p 4 app/build/outputs/apk/release/app-release-unsigned.apk app-release-unsigned-aligned.apk'
        withCredentials([string(credentialsId: 'testKeystorePassword', variable: 'STORE_PASS'), string(credentialsId: 'keyPassword', variable: 'KEY_PASS')]) {
          sh '$ANDROID_HOME/build-tools/28.0.3/apksigner sign --ks /Users/kien/keystores/kienTestKeyStore.jks --ks-pass pass:$STORE_PASS --ks-key-alias JenkinsAndroidTest --key-pass pass:$KEY_PASS --out JenkinsAndroid-release.apk app-release-unsigned-aligned.apk'
        }
      }
      post {
        success {
          archiveArtifacts(fingerprint: true, artifacts: 'JenkinsAndroid-release.apk')
        }
      }
    }
  }
}