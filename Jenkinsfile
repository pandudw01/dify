pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'docker_username'  
        DOCKER_REGISTRY = 'https://index.docker.io/v1/' 
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out from Git'
                checkout scm
            }
        }
        stage('Verify Tools') {
            steps {
                script {
                    if (isUnix()) {
                        sh '''
                        docker --version
                        aws --version
                        '''
                    } else {
                        echo 'Tool verification script only runs on Unix-based systems'
                    }
                }
            }
        }
        stage('Build API and Web Image') {
            steps {
                script {
                    sh '''
                    docker pull nginx
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed, man'
        }
    }
}
