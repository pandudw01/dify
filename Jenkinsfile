pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'docker_username'  
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
                    make build-all
                    '''
                }
            }
        }
        stage('Docker Login and Push') {
            steps {
                script {
                    docker.withRegistry(DOCKER_REGISTRY, DOCKER_CREDENTIALS_ID) {
                        echo 'Logged in to Docker Hub'
                        
                        sh '''
                        make push-all
                        '''

                        echo 'Push completed'
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed, guys'
        }
    }
}
