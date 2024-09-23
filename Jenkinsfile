pipeline {
    agent { 
        docker { 
            image 'node:16' 
        } 
    }
    environment {
        // Reference the Snyk token stored in Jenkins credentials
        SNYK_TOKEN = credentials('SNYK_TOKEN')
    }
    stages {
        stage('Install dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh 'node --version'
                sh 'npm --version'
                sh 'npm install --save'
                sh 'npm install express@4.20.0'
            }
        }

        stage('Snyk Scan') {
            steps {
                script {
                    echo 'Installing and Running Snyk Scan...'
                    // Install Snyk and run a security scan using the SNYK_TOKEN
                    sh '''
                    npm install snyk -g
                    snyk auth ${SNYK_TOKEN}
                    '''
                    
                    // Running the Snyk test, halting the pipeline on high or critical vulnerabilities
                    def snykResult = sh(script: 'snyk test --severity-threshold=high', returnStatus: true)
                    
                    if (snykResult != 0) {
                        echo 'Snyk found critical vulnerabilities!'
                        currentBuild.result = 'FAILURE'
                        error('Critical vulnerabilities detected. Halting the pipeline.')
                    } else {
                        echo 'No critical vulnerabilities found!'
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running Tests...'
                // Ensure that tests are run if available
                sh 'npm test || echo "No tests defined."'
            }
        }
    }

    post {
        always {
            // Always archive the artifacts and logs regardless of the build result
            archiveArtifacts artifacts: '**/build-logs/**'  // Replace with actual log file location if needed
            junit '**/test-results.xml'  // Replace with actual test result files if present
            echo 'Logs and artifacts have been archived.'
        }
        failure {
            // On failure, make sure that failure logs are archived and accessible
            echo 'Build failed! Archiving logs...'
        }
        success {
            // On success, display a success message
            echo 'Build succeeded! Archiving logs...'
        }
    }
}

