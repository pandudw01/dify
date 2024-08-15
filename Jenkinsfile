pipeline {
    agent any

    // tools {
    
    // }

    // environment {

    // }

    // triggers {
    //     githubPush(triggerOnPush: true, triggerOnMergeRequest: true, branchFilterType: 'All')
    // }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out from Git'
                checkout scm
            }
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
