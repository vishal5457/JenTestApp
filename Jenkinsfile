pipeline {
    agent any

    tools {
        maven 'maven'
        jdk 'java'
    }

    environment {
        DOCKER_HUB = "your-dockerhub-username"
        IMAGE_NAME = "my-app"
    }

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/your-username/my-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube') {
            steps {
                sh '''
                mvn sonar:sonar \
                -Dsonar.projectKey=my-app \
                -Dsonar.host.url=http://<SONAR-IP>:9000 \
                -Dsonar.login=<TOKEN>
                '''
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
                sh 'docker run -d -p 8081:8080 --name my-app $DOCKER_HUB/$IMAGE_NAME:latest'
            }
        }
    }
}
