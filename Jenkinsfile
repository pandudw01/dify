pipeline {
    agent {
        docker { image 'python:3.10-alpine' }
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out from Git'
                checkout scm 
            }
        }
        stage('Setup Docker') {
            steps {
                script {
                    sh '''
                    docker --version
                    '''
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    sh '''
                    docker pull hello-world
                    docker run hello-world
                    '''
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline selesai'
        }
    }
}
