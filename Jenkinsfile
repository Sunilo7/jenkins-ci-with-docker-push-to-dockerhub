pipeline {
    agent any

    tools {
        jdk 'java-11'
        maven 'maven'
    }

    stages {

        stage('Git Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/rashmigmr13-eng/CI.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t rashmidevops1/ci-app:1 .'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {
                    sh '''
                    echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                sh 'docker push rashmidevops1/ci-app:1'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop c1 || true
                docker rm c1 || true
                docker run -d --name c1 -p 9000:8080 rashmidevops1/ci-app:1
                '''
            }
        }
    }
}
