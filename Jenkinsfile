pipeline {
    agent {
        docker {
            image 'python:3.10-alpine' 
            args '-v /var/run/docker.sock:/var/run/docker.sock' 
        }
    }
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm /
            }
        }
        stage('Test') {
            steps {
                sh 'python --version'
            }
        }
    }
    post {
        always {
            echo 'Completed'
        }
    }
}
