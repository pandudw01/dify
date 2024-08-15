pipeline {
    agent any

    environment {
        WORKING_DIRECTORY = 'sdks/nodejs-client' 
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out from Git'
                checkout scm
            }
        }

        stage('Setup Docker') {
            steps{
                echo "Set up Docker"
                sh 'curl https://get.docker.com/ -o install-docker.sh'
                chmod +x install-docker.sh
                ./install-docker.sh
                sudo usermod -aG docker $USER 
                newgrp docker
                source ~/.bashrc
            }
        }

        stage('Test Matrix') {
            matrix {
                axes {
                    axis {
                        name 'NODE_VERSION'
                        values '16', '18', '20'
                    }
                }
                stages {
                    stage('Setup Node.js') {
                        steps {
                            script {
                                docker.image("node:${env.NODE_VERSION}").inside {
                                    dir("${WORKING_DIRECTORY}") {
                                        // Instal dependencies
                                        sh 'yarn install'
                                    }
                                }
                            }
                        }
                    }

                    stage('Run Tests') {
                        steps {
                            script {
                                docker.image("node:${env.NODE_VERSION}").inside {
                                    dir("${WORKING_DIRECTORY}") {
                                        // Jalankan unit tests
                                        sh 'yarn test'
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Tests passed successfully!'
        }
        failure {
            echo 'Tests failed.'
        }
    }
}
