pipeline {
    agent any

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
                        echo 'Docker installation script only runs on Unix-based systems'
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
