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
                sh 'docker build -t rashmigmr13/ci-app:1 .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop c1 || true
                docker rm c1 || true
                docker run -d --name c1 -p 9000:8080 rashmigmr13/ci-app:1
                '''
            }
        }
    }
}
