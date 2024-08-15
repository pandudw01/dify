pipeline {
    agent none

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out from Git'
                checkout scm 
            }
        }
    //     stage('Test') {
    //         steps {
    //             sh 'node --version' 
    //         }
    //     }
    // }
    
    // post {
    //     always {
    //         echo 'Pipeline selesai'
    //     }
    // }
}
}