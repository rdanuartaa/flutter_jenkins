pipeline {
    agent any

    environment {
        // Path Flutter & Android SDK
        FLUTTER_HOME = '/opt/flutter'
        ANDROID_HOME = '/opt/android-sdk'
        ANDROID_SDK_ROOT = '/opt/android-sdk'  // Tambahkan ini juga
    }

    stages {
        stage('🔍 Verify Environment') {
            steps {
                sh '''
                    echo "=== Environment Check ==="
                    echo "FLUTTER_HOME: ${FLUTTER_HOME}"
                    echo "ANDROID_HOME: ${ANDROID_HOME}"
                    echo "PATH: $PATH"
                    ${FLUTTER_HOME}/bin/flutter --version
                '''
            }
        }

        stage('📦 Get Dependencies') {
            steps {
                dir('register_system/backend/flutter_app') {
                    sh '''
                        ${FLUTTER_HOME}/bin/flutter pub get
                    '''
                }
            }
        }

        stage('🔨 Build APK Release') {
            steps {
                dir('register_system/backend/flutter_app') {
                    sh '''
                        # ⚠️ PENTING: Export environment variables secara eksplisit
                        export FLUTTER_HOME="${FLUTTER_HOME}"
                        export ANDROID_HOME="${ANDROID_HOME}"
                        export ANDROID_SDK_ROOT="${ANDROID_SDK_ROOT}"
                        export PATH="${FLUTTER_HOME}/bin:${ANDROID_HOME}/cmdline-tools/latest/bin:${ANDROID_HOME}/platform-tools:$PATH"
                        
                        echo "🧹 Cleaning..."
                        ${FLUTTER_HOME}/bin/flutter clean
                        
                        echo "📦 Getting dependencies..."
                        ${FLUTTER_HOME}/bin/flutter pub get
                        
                        echo "🔨 Building APK..."
                        ${FLUTTER_HOME}/bin/flutter build apk --release
                        
                        echo "✅ Build complete!"
                        ls -lh build/app/outputs/flutter-apk/*.apk || echo "⚠️ APK not found"
                    '''
                }
            }
        }

        stage('📤 Archive APK') {
            steps {
                archiveArtifacts artifacts: 'register_system/backend/flutter_app/build/app/outputs/flutter-apk/*.apk', 
                                 allowEmptyArchive: false,
                                 fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ Flutter APK build success! Download dari Artifacts section.'
        }
        failure {
            echo '❌ Build failed! Check console output.'
        }
        always {
            dir('register_system/backend/flutter_app') {
                sh '${FLUTTER_HOME}/bin/flutter clean || true'
            }
        }
    }
}