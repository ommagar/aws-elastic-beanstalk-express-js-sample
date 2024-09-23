pipeline {
    agent { docker { image 'node:16' } }
    stages {
        stage('Install dependencies') {
            steps {
                sh 'node --version'
                sh 'npm --version'
                sh 'npm install --save'
                sh 'npm install express@4.20.0'
            }
        }
        stage('Snyk Scan') {
            steps {
                echo 'Scanning...'
                sh '''
                npm install snyk -g
                snyk auth 6c728dac-904d-43e3-a42a-723c5623a8b0
                snyk test --severity-threshold=high
                '''
            }
        }
    }
}
