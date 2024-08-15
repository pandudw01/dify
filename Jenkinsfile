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
                        echo "Updating package lists"
                        sudo apt-get update -y

                        echo "Installing dependencies"
                        sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common

                        echo "Adding Docker's official GPG key"
                        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

                        echo "Setting up the Docker repository"
                        sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

                        echo "Updating package lists again"
                        sudo apt-get update -y

                        echo "Installing Docker"
                        sudo apt-get install -y docker-ce

                        echo "Verifying Docker installation"
                        sudo docker --version
                        sudo usermod -aG docker $USER
                        newgrp docker
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
            echo 'Pipeline completed'
        }
    }
}
