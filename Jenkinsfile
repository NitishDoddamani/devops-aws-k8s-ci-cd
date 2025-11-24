pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-creds'   // Jenkins credentials ID
        DOCKER_IMAGE = 'nitishdoddamani/sample-node-app'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/NitishDoddamani/devops-aws-k8s-ci-cd.git'
            }
        }

        stage('Build app') {
            steps {
                dir('app') {
                    bat 'npm install'
                    bat 'npm test || echo "no tests yet"'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('app') {
                    script {
                        bat "docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIALS,
                                                     usernameVariable: 'DOCKER_USER',
                                                     passwordVariable: 'DOCKER_PASS')]) {
                        bat "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                        bat "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                    }
                }
            }
        }

        // Later we will add 'Deploy to EKS' stage here
    }
}
