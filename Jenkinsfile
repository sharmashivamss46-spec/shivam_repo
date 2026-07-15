pipeline {
    agent any

    environment {
        IMAGE_NAME = "sharmashivamss46/java-webapp"
        TAG = "${BUILD_NUMBER}"

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/sharmashivamss46-spec/shivam_repo.git'
            }
        }

        stage('Build Maven Project') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${IMAGE_NAME}:${TAG} .
                docker tag ${IMAGE_NAME}:${TAG} ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'sharmashivamss46',
                    passwordVariable: 'Iphone@123456789'
                )]) {

                    sh '''
                    echo ${DOCKER_PASS} | docker login -u ${DOCKER_USER} --password-stdin

                    docker push ${IMAGE_NAME}:${TAG}
                    docker push ${IMAGE_NAME}:latest

                    docker logout
                    '''
                }
            }
        }

        stage('Deploy Docker Containers') {
            steps {
                sh '''
                docker rm -f app1 app2 || true

                docker run -d --name app1 -p 8081:8080 ${IMAGE_NAME}:${TAG}

                docker run -d --name app2 -p 8082:8080 ${IMAGE_NAME}:${TAG}
                '''
            }
        }

    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed.'
        }

        always {
            cleanWs()
        }
    }
}
