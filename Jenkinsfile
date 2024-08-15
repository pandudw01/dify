pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out from Git'
                checkout scm 
            }
        }
    }
        stage('Setup Docker') {
            steps {
                script {
                    sh '''
                    sudo apt-get update -y
                    sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
                    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
                    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
                    sudo apt-get update -y
                    sudo apt-get install -y docker-ce
                    sudo docker --version
                    '''
                }
            }
        }
}