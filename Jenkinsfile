pipeline {
    agent any

    // tools {
    
    // }

    // environment {

    // }

    triggers {
        githubPush(triggerOnPush: true, triggerOnMergeRequest: true, branchFilterType: 'All')
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out from Git'
                checkout scm
            }
        }
    }
}