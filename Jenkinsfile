pipeline {
    agent any

    environment {
        WORKING_DIRECTORY = 'sdks/nodejs-client' // Direktori kerja default
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out from Git'
                checkout scm
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
