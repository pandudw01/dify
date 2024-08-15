pipeline {
    agent {
        docker {image 'node:20.16.0-alpine3.20'}
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out from Git'
                checkout scm
            }
        }
    }
    stages {
        stage('Test') {
            steps {
                sh 'node --version'
            }
        }
    }   
}