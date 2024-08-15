pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'docker-credentials'  
        DOCKER_REGISTRY = 'docker.io' 
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
                    docker.withRegistry(DOCKER_REGISTRY, DOCKER_CREDENTIALS_ID) {
                        echo 'Logged in to Docker Registry'

                        sh '''
                        make build-push-all
                        '''
                    }
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
