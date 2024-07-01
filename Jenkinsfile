pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('my-docker-image', '-f Dockerfile .')
                }
            }
        }
    }
}
