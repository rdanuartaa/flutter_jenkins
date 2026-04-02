pipeline {
    agent any
    
    stages {
        stage('Build APK') {
            steps {
                dir('register_system/backend/flutter_app') {
                    sh '''
                        /opt/flutter/bin/flutter clean
                        /opt/flutter/bin/flutter pub get
                        /opt/flutter/bin/flutter build apk --release
                    '''
                }
            }
        }
        stage('Archive APK') {
            steps {
                archiveArtifacts artifacts: 'register_system/backend/flutter_app/build/app/outputs/flutter-apk/*.apk'
            }
        }
    }
    post {
        success {
            echo '✅ Build success! APK ready to download.'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}