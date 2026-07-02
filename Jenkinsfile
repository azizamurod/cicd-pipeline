pipeline {
    agent any

    environment {
        PATH = "/opt/homebrew/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'CI=true npm test -- --watchAll=false'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t react-app:${BRANCH_NAME} .'
            }
        }
    }
}