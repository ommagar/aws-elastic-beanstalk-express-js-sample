pipeline {
    agent {
        docker {
            image 'node:16'
            label 'docker'
        }
    }
    environment {
        SNYK_TOKEN = credentials('SNYK_TOKEN')
        LOG_FILE = 'pipeline.log'  // Define a log file name
    }
    stages {
        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Installing dependencies...' | tee -a "${LOG_FILE}"  // Log to file
                    sh 'npm install --save 2>&1 | tee -a "${LOG_FILE}"'  // Log output
                }
            }
        }
        stage('Install Snyk') {
            steps {
                script {
                    echo 'Installing Snyk...' | tee -a "${LOG_FILE}"  // Log to file
                    sh 'npm install -g snyk 2>&1 | tee -a "${LOG_FILE}"'  // Log output
                }
            }
        }
        stage('Snyk Security Scan') {
            steps {
                script {
                    echo 'Running Snyk security scan...' | tee -a "${LOG_FILE}"  // Log to file
                    sh 'snyk auth $SNYK_TOKEN 2>&1 | tee -a "${LOG_FILE}"'  // Log output
                    sh 'snyk test || true 2>&1 | tee -a "${LOG_FILE}"'  // Log output and prevent failure
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    echo 'Running tests...' | tee -a "${LOG_FILE}"  // Log to file
                    sh 'npm test 2>&1 | tee -a "${LOG_FILE}"'  // Log output
                }
            }
        }
    }
    post {
        failure {
            script {
                echo 'Build failed!' | tee -a "${LOG_FILE}"  // Log failure message
            }
        }
        success {
            script {
                echo 'Build succeeded!' | tee -a "${LOG_FILE}"  // Log success message
            }
        }
    }
}

