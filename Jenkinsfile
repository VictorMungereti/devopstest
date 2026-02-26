pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "your-dockerhub-username/sample-app"
        DOCKER_TAG = "${BUILD_NUMBER}

    tools {
        maven "Maven-3.9"
        jdk "JDK-17"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Cloning repository..."
                git branch: 'main', 
                    url: 'https://github.com/your-username/sample-app.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building the project..."
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo "Packaging application..."
                sh 'mvn package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image..."
                    sh "docker build -t $DOCKER_IMAGE:$DOCKER_TAG ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    echo "Pushing Docker image..."
                    withCredentials([string(credentialsId: 'dockerhub-password', variable: 'DOCKER_PASS')]) {
                        sh """
                        echo $DOCKER_PASS | docker login -u your-dockerhub-username --password-stdin
                        docker push $DOCKER_IMAGE:$DOCKER_TAG
                        """
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying application..."
                sh """
                docker stop sample-app || true
                docker rm sample-app || true
                docker run -d -p 8080:8080 --name sample-app $DOCKER_IMAGE:$DOCKER_TAG
                """
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}