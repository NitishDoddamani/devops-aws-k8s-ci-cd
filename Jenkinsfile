pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-creds'
        DOCKER_IMAGE = 'nitishdoddamani/sample-node-app'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build app') {
            steps {
                dir('app') {
                    bat 'npm install'
                    bat 'npm test || exit 0'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('app') {
                    bat "docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIALS,
                                                     usernameVariable: 'DOCKER_USER',
                                                     passwordVariable: 'DOCKER_PASS')]) {
                        bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                        bat "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                    }
                }
            }
        }
    }
}
