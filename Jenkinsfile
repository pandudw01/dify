pipeline {
    agent none
    environment {
        DOCKERHUB_USER = credentials('dockerhub_credentials') 
        DOCKERHUB_TOKEN = credentials('dockerhub_credentials') 
        DIFY_WEB_IMAGE_NAME = "${DIFY_WEB_IMAGE_NAME ?: 'langgenius/dify-web'}"
        DIFY_API_IMAGE_NAME = "${DIFY_API_IMAGE_NAME ?: 'langgenius/dify-api'}"
    }
    stages {
        stage('Prepare') {
            agent {
                label 'linux' // atau label yang sesuai untuk runner Anda
            }
            matrix {
                axes {
                    axis {
                        name 'PLATFORM'
                        values 'linux/amd64', 'linux/arm64'
                    }
                    axis {
                        name 'CONTEXT'
                        values 'api', 'web'
                    }
                }
                stages {
                    stage('Build Docker image') {
                        steps {
                            script {
                                def imageName = env.PLATFORM.contains('arm64') ? "${DIFY_API_IMAGE_NAME}" : "${DIFY_WEB_IMAGE_NAME}"
                                def context = env.CONTEXT
                                def platform = env.PLATFORM
                                def serviceName = "build-${context}-${platform}"

                                echo "Building ${context} image for platform ${platform}"

                                // Instal Docker dan setup Docker environment
                                sh 'curl -fsSL https://get.docker.com/ | sudo sh'
                                sh 'sudo usermod -aG docker $USER'
                                sh 'newgrp docker || true'

                                docker.withRegistry('https://index.docker.io/v1/', 'dockerhub_credentials') {
                                    def buildArgs = [
                                        '--build-arg', "COMMIT_SHA=${env.GIT_COMMIT}"
                                    ]
                                    def cacheFrom = "type=gha,scope=${serviceName}"
                                    def cacheTo = "type=gha,mode=max,scope=${serviceName}"

                                    docker.build(imageName, "-f Dockerfile.${context} --platform ${platform} ${buildArgs.join(' ')} .")
                                    docker.image(imageName).push('latest')
                                }
                            }
                        }
                    }
                }
            }
        }
        stage('Create Manifest') {
            agent {
                label 'linux' // atau label yang sesuai untuk runner Anda
            }
            steps {
                script {
                    def apiImageName = "${DIFY_API_IMAGE_NAME}"
                    def webImageName = "${DIFY_WEB_IMAGE_NAME}"

                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub_credentials') {
                        sh 'docker buildx create --use'
                        sh 'docker buildx inspect --bootstrap'

                        sh """
                        docker buildx imagetools create \
                        ${apiImageName} \
                        ${webImageName}
                        """
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Cleaning up...'
        }
    }
}
