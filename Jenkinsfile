
pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Shubhada4141/final-todo.git'
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t shubhadashingane/final_todo_last:latest .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {
                    sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh 'docker push shubhadashingane/final_todo_last:latest'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker stop final-todo || true
                    docker rm final-todo || true
                    docker pull shubhadashingane/final_todo_last:latest
                    docker run -d --name final-todo -p 8080:8080 shubhadashingane/final_todo_last:latest
                '''
            }
        }
    }
}
