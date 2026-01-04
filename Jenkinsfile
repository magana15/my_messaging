pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('message') {
                    sh '''
                    docker build -t messaging-app .
                    '''
                }
            }
        }

        stage('Run Tests') {
            steps {
                dir('message') {
                    sh '''
                    docker run --rm messaging-app pytest --html=report.html --self-contained-html
                    '''
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'message/report.html', fingerprint: true
        }
    }
}
