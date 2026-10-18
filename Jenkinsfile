pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jk-static-website:latest .'
            }
        }

        stage('Deploy Website') {
            steps {
                sh '''
                    docker rm -f jk-static-website || true

                    docker run -d \
                      --name jk-static-website \
                      --restart unless-stopped \
                      -p 8081:80 \
                      jk-static-website:latest
                '''
            }
        }

        stage('Verify Website') {
            steps {
                sh 'docker ps'
                sh 'curl --fail http://localhost:8081/'
            }
        }
    }
}
