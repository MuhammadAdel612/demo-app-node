pipeline {
    agent any

    environment {
        DOCKER_USER = 'MuhammadAdel8'
        IMAGE_NAME = 'my-app'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Buildiing New Image'
                sh '''
                 docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                '''
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                  withcredentials([usernamePasword](
                    credentialsId: 'dockerid'
                    usernameVariable: 'Username'
                    userpassVariable: 'Password'
                    )]) {
                    echo 'login to docker'
                    sh "echo \$DOCKER_HUB_PASSWORD | docker login -u \$Username --password-stdin"
                    sh 'docker push ${DOCKER_USER}/${IMAGE_NAME}:${IMAGE_TAG}'
                    }
                }
            }
        }
        }
