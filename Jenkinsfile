pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing project dependencies...'
                // Prefer npm ci for CI reproducibility; use bat on Windows agents
                script {
                    if (isUnix()) {
                        sh 'npm ci'
                    } else {
                        bat 'npm ci'
                    }
                }
            }
        }

        stage('Validate student.json Data & Form Rules') {
            steps {
                echo 'Starting automated test suite...'
                script {
                    if (isUnix()) {
                        sh 'node test-runner.js'
                    } else {
                        bat 'node test-runner.js'
                    }
                }
            }
        }
    }

    post {
        success {
            echo '🎉 All student.json test cases validated successfully!'
        }
        failure {
            echo '🚨 One or more test cases failed validation checks.'
        }
    }
}
