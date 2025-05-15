pipeline {
    agent any

    tools {
        nodejs 'nodejs' // Ensure this matches the tool name in Jenkins
    }

    environment {
        EMAIL_TO = 'sonis95190618@gmail.com'
        EMAIL_TO_SECURITY = 'bobbyverma2001@example.com'
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
                    def testStatus = sh(script: 'npm test > test.log 2>&1', returnStatus: true)
                    if (testStatus != 0) {
                        currentBuild.result = 'FAILURE'
                        error "Tests failed"
                    }
                }
            }
            post {
                always {
                    emailext (
                        subject: "Build ${env.JOB_NAME} #${env.BUILD_NUMBER} - Test Stage: ${currentBuild.currentResult}",
                        body: "Test stage completed with status: ${currentBuild.currentResult}\n\nCheck the test log.",
                        to: "${env.EMAIL_TO}",
                        attachmentsPattern: 'test.log',
                        mimeType: 'text/plain'
                    )
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    def auditStatus = sh(script: 'npm audit --json > audit-report.json', returnStatus: true)
                    sh 'npm audit > audit.log 2>&1'
                    if (auditStatus != 0) {
                        echo "Security vulnerabilities found."
                    }
                }
            }
            post {
                always {
                    emailext (
                        subject: "Build ${env.JOB_NAME} #${env.BUILD_NUMBER} - Security Scan: ${currentBuild.currentResult}",
                        body: "Security scan completed with status: ${currentBuild.currentResult}\n\nCheck audit.log for details.",
                        to: "${env.EMAIL_TO_SECURITY}",
                        attachmentsPattern: 'audit.log',
                        mimeType: 'text/plain'
                    )
                }
            }
        }
    }
}
