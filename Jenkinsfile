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
                    sh 'snyk auth 6c728dac-904d-43e3-a42a-723c5623a8b0'
                    // This command will fail the build if critical vulnerabilities are detected
                    def snykResult = sh(script: 'snyk test', returnStatus: true)
                    if (snykResult != 0) {
                        echo 'Snyk found vulnerabilities!'
                        currentBuild.result = 'FAILURE'  // Mark the build as failure
                        error('Critical vulnerabilities detected. Halting the pipeline.')
                    }
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

