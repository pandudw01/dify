pipeline {
    agent {
        docker { image 'python:3.10-alpine' } 
    }
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm 
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
            echo 'Pipeline selesai'
        }
    }
}
