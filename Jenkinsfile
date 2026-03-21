pipeline {
    agent any

    tools {
        maven 'maven'
        jdk 'java'
    }

    environment {
        DOCKER_HUB = "vishal4089"
        IMAGE_NAME = "my-app"
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    
    stages {

        stage('Sonar test') {
            steps {
                sh 'mvn sonar:sonar'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $DOCKER_HUB/$IMAGE_NAME:latest .'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push $DOCKER_HUB/$IMAGE_NAME:latest'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker rm -f my-app || true'
                sh '''
                docker run -d \
                -p 8081:8080 \
                --restart unless-stopped \
                --name my-app \
                $DOCKER_HUB/$IMAGE_NAME:latest
                '''
            }
        }
    }
}
