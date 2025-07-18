pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-cred')
        GIT_CREDENTIALS = credentials('github-cred')
        IMAGE_NAME = "bharathi136/dockerhub-build-pipeline"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: 'github-cred', url: 'https://github.com/bharathi1306/dockerhub-build-pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                script {
                    sh "echo ${DOCKER_HUB_CREDENTIALS_PSW} | docker login -u ${DOCKER_HUB_CREDENTIALS_USR} --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.image("${IMAGE_NAME}:${BUILD_NUMBER}").push()
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
