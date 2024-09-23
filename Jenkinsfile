pipeline {
    agent {
        docker {
            image 'node:16'
        }
    }
    stages {
        stage('Install dependencies') {
            steps {
                script {
                    sh 'npm install --save'
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}

