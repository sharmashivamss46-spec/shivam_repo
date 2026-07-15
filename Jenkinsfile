pipeline {

    agent any

    environment {
        IMAGE_NAME = "yourdockerhubusername/java-webapp"
        TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/sharmashivamss46-spec/shivam_repo.git/java-webapp.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'sharmashivamss46',
                    passwordVariable: 'Iphone@123456789'
                )]) {

                    sh '''
                    echo $PASSWORD | docker login -u $USERNAME --password-stdin
                    docker push $IMAGE_NAME:$TAG
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f app1 app2 || true

                docker run -d --name app1 -p 8081:8080 $IMAGE_NAME:$TAG

                docker run -d --name app2 -p 8082:8080 $IMAGE_NAME:$TAG
                '''
            }
        }

    }
}
