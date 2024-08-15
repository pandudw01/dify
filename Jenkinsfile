pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out from Git'
                checkout scm
            }
        }
        stage('Install Docker') {
            steps {
                script {
                    if (isUnix()) {
                        sh '''
                        docker --version
                        '''
                    } else {
                        echo 'Docker installation script only runs on Unix-based systems'
                    }
                }
            }
        }
        stage('Verify Docker') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'docker --version'
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
