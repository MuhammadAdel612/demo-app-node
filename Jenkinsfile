pipeline {
    agent any

    environment {
        DOCKER_USER = 'MuhammadAdel8'
        IMAGE_NAME = 'my-app'
        IMAGE_TAG = "${BUILD_NUMBER}"
        REGISTRY_CREDS = 'dockerid' // Your global Jenkins credential ID
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
                echo 'Building New Image'
                // Tagged with your Docker Hub user so it can be pushed properly
                sh '''
                docker build -t ${DOCKER_USER}/${IMAGE_NAME}:${IMAGE_TAG} .
                docker tag ${DOCKER_USER}/${IMAGE_NAME}:${IMAGE_TAG} ${DOCKER_USER}/${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo 'Logging in and pushing to Docker Hub...'
                script {
                    // This securely injects the 'dockerid' credential values into temporary variables
                    withCredentials([usernamePassword(credentialsId: "${REGISTRY_CREDS}", 
                                                     usernameVariable: 'DOCKER_HUB_USER', 
                                                     passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                        
                        // Login using the temporary variables
                        sh "echo \$DOCKER_HUB_PASSWORD | docker login -u \$DOCKER_HUB_USER --password-stdin"
                        
                        // Push both the build number tag and the latest tag
                        sh "docker push ${DOCKER_USER}/${IMAGE_NAME}:${IMAGE_TAG}"
                        sh "docker push ${DOCKER_USER}/${IMAGE_NAME}:latest"
                    }
                }
            }
        }
    }
    
    post {
        always {
            echo 'Cleaning up local Docker images...'
            // Best practice: logout and delete local images to keep your Jenkins server clean
            sh "docker logout"
            sh "docker rmi ${DOCKER_USER}/${IMAGE_NAME}:${IMAGE_TAG} || true"
            sh "docker rmi ${DOCKER_USER}/${IMAGE_NAME}:latest || true"
        }
    }
}
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
    }

        }
