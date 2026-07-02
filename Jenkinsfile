pipeline {
    agent any

    environment {
        PATH = "/usr/local/bin:/opt/homebrew/bin:/usr/bin:/bin:/usr/sbin:/sbin"
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

        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == "main") {
                        sh '''
                        docker rm -f react-main || true
                        docker run -d --name react-main -p 3000:3000 react-app:main
                        '''
                    } else {
                        sh '''
                        docker rm -f react-dev || true
                        docker run -d --name react-dev -p 3001:3000 react-app:dev
                        '''
                    }
                }
            }
        }
    }
}