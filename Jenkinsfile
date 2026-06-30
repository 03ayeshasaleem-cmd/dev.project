pipeline {
    agent any

    environment {
        IMAGE_NAME = "03ayesha/devops-project:v1"
    }

    stages {

        stage('Clone') {
            steps {
                echo 'Starting Pipeline'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t %IMAGE_NAME% ."
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'USERNAME',
                    passwordVariable: 'PASSWORD'
                )]) {

                    sh 'docker login -u %USERNAME% -p %PASSWORD%'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh "docker push %IMAGE_NAME%"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}