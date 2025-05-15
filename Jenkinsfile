pipeline {
    agent any

    tools {
        nodejs 'nodejs' // Your configured Node.js tool in Jenkins
    }

    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/Bobby032001/nodejs-goof.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run tests, save output to a log file
                    def testStatus = sh(script: 'npm test > test.log 2>&1', returnStatus: true)
                    if (testStatus != 0) {
                        error "Tests failed"
                    }
                }
            }
            post {
                always {
                    emailext (
                        subject: "Build ${currentBuild.fullDisplayName} - Test Stage: ${currentBuild.currentResult}",
                        body: "The Test stage has completed with status: ${currentBuild.currentResult}",
                        to: 'developer@example.com',
                        attachmentsPattern: 'test.log'
                    )
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    // Run npm audit, save output in JSON and log formats
                    def auditStatus = sh(script: 'npm audit --json > audit-report.json', returnStatus: true)
                    sh 'npm audit > audit.log 2>&1'
                    if (auditStatus != 0) {
                        echo "Vulnerabilities found during security scan."
                    }
                }
            }
            post {
                always {
                    emailext (
                        subject: "Build ${currentBuild.fullDisplayName} - Security Scan: ${currentBuild.currentResult}",
                        body: "The Security Scan stage has completed with status: ${currentBuild.currentResult}",
                        to: 'bobbyverma2001@example.com',
                        attachmentsPattern: 'audit.log'
                    )
                }
            }
        }
    }
}
