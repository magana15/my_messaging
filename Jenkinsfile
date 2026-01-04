pipeline {
    agent any

    environment {
        IMAGE_NAME = "messaging-app"
        GIT_COMMIT = "${env.GIT_COMMIT.take(7)}"
        DOCKER_TAG = "${IMAGE_NAME}:${GIT_COMMIT}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('message') {
                    sh """
                    echo "Building Docker image ${DOCKER_TAG}..."
                    docker build -t ${DOCKER_TAG} .
                    """
                }
            }
        }

        stage('Run Tests in Docker') {
            steps {
                dir('message') {
                    sh """
                    echo "Running tests inside Docker container..."
                    docker run --rm ${DOCKER_TAG} pytest --html=report.html --self-contained-html
                    """
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'message/report.html', fingerprint: true
            echo "Artifacts archived."
        }
        success {
            echo "Pipeline succeeded for ${DOCKER_TAG}!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
