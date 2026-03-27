pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDS = credentials('docker-hub-credentials')
    }

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/prabhanshulohani/test'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t usermanagement-application-image:latest .'
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag usermanagement-application-image:latest prabhanshu450/usermanagement:latest'
            }
        }

        stage('Push Image') {
            steps {
                sh 'echo $DOCKER_HUB_CREDS_PSW | docker login -u $DOCKER_HUB_CREDS_USR --password-stdin'
                sh 'docker push prabhanshu450/usermanagement:latest'
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                sh 'docker compose up -d'
            }
        }

        stage('Clean Workspace') {
            steps {
                sh 'rm -rf *'
            }
        }
    }
}
