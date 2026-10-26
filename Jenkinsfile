pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Image') {
            steps {
                sh 'docker build -t angelbrownr/todoapp:latest .'
            }
        }
        
        stage('Test Run') {
            steps {
                sh '''
                  docker rm -f todo-app 2>/dev/null || true
                  docker run -d --name todo-app -p 3100:3000 angelbrownr/todoapp:latest
                  sleep 5
                  curl -f http://localhost:3100 || true
                  docker rm -f todo-app 2>/dev/null || true
                '''
            }
        }
        
        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                      echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                      docker push angelbrownr/todoapp:latest
                      docker logout
                    '''
                }
            }
        }
        
        stage('Deploy') {
            steps {
                sh '''
                  docker rm -f todo-app 2>/dev/null || true
                  docker run -d --name todo-app --restart unless-stopped -p 3000:3000 angelbrownr/todoapp:latest
                '''
            }
        }
    }
}