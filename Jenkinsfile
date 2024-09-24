pipeline {
    agent {
        docker {
            image 'node:16'
            label 'docker' // Ensure this label is correctly assigned to your Jenkins agent
        }
    }
    environment {
        SNYK_TOKEN = credentials('SNYK_TOKEN')
    }
    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Installing dependencies...'
                    sh 'npm install --save > pipeline.log 2>&1'
                }
            }
        }
        stage('Install Snyk') {
            steps {
                script {
                    echo 'Installing Snyk...'
                    sh 'npm install -g snyk >> pipeline.log 2>&1'
                }
            }
        }
        stage('Snyk Security Scan') {
            steps {
                script {
                    echo 'Running Snyk security scan...'
                    sh 'snyk auth $SNYK_TOKEN >> pipeline.log 2>&1'
                    sh 'snyk test || true >> pipeline.log 2>&1'  // Prevent build failure on vulnerabilities
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    sh 'npm test >> pipeline.log 2>&1'  // Modify as needed
                }
            }
        }
    }
    post {
        failure {
            script {
                echo 'Build failed!'
            }
        }
        success {
            script {
                echo 'Build succeeded!'
            }
        }
        always {
            // Archive the log file regardless of success or failure
            archiveArtifacts artifacts: 'pipeline.log', fingerprint: true
        }
    }
}

