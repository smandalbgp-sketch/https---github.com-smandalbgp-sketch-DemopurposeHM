pipeline {
    agent any

environment {
    IMAGE_NAME = 'smandalbgp-sketch-app'
    PATH = "/Users/aps/.nvm/versions/node/v24.18.0/bin:/usr/local/bin:${env.PATH}"
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

        stage('Build Angular App') {
            steps {
                sh 'npm run build -- --configuration=production'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Deploy') {
    steps {
        sh '''
            docker stop hospital-app || true
            docker rm hospital-app || true
            docker run -d --name hospital-app -p 8081:80 smandalbgp-sketch-app:${BUILD_NUMBER}
        '''
    }
}

    post {
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed.'
        }
    }
}
