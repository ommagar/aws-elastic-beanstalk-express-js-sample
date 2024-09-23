pipeline {
    agent {
        docker {
            image 'node:16'
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
                    sh 'npm install --save'
                }
            }
        }
        stage('Install Snyk') {
            steps {
                script {
                    echo 'Installing Snyk...'
                    sh 'npm install -g snyk'
                }
            }
        }
        stage('Snyk Security Scan') {
            steps {
                script {
                    echo 'Running Snyk security scan...'
                    sh 'snyk auth $SNYK_TOKEN'
                    sh 'snyk test || true'  // Prevent build failure on vulnerabilities
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    // If you have tests defined, use npm test. Otherwise, skip or remove this.
                    sh 'npm test'  // Remove or modify this line if no tests are defined
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
    }
}
