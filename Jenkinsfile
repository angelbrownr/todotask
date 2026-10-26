pipeline {
    agent any
    environment {
        PATH = "/Users/admin/.docker/bin:/Applications/Docker.app/Contents/Resources/bin:/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:$PATH"
    }
    stages {
        stage('Build Image') {
            steps {
                sh '''
                which docker
                docker --version
                docker build -t brownangelr/todoapp:latest .
                '''
            }
        }
        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push brownangelr/todoapp:latest'
                }
            }
        }
    }
}