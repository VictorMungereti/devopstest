pipeline {
    agent any

    environment {
        APP_NAME = "my-app"
        BUILD_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Build') {
            steps {
                echo "Building ${APP_NAME}"
                echo "Build number is ${BUILD_TAG}"
                sh 'echo Build stage running...'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'echo Tests passed!'
            }
        }

    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed."
        }
    }
}