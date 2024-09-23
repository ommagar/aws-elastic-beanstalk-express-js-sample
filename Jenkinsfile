pipeline {
    agent {
        docker {
            image 'node:16'
            args '-u root -v /var/jenkins_home:/var/jenkins_home'  // Ensure root user and volume mount
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
                    def snykResult = sh(script: 'snyk test', returnStatus: true)
                    if (snykResult != 0) {
                        echo 'Snyk found vulnerabilities!'
                        currentBuild.result = 'FAILURE'  // Mark the build as failure
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    echo 'Running tests...'
                    sh 'npm test'  // Modify or remove this line if no tests are defined
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

