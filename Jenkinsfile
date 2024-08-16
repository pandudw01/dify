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
                        sh '''
                        docker --version
                        aws --version
                        '''
                        echo 'Tool verification script only runs on Unix-based systems'
                    
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
                        
                    sh '''
                    make push-all
                    '''

                    echo 'Push completed'
                    
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
