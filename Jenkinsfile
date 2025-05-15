pipeline {
    agent {
        node {
            label 'master'
        }
    }

    tools {
        nodejs 'nodejs'
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

        stage('Security Scan') {
            steps {
                script {
                    def auditStatus = sh(script: 'npm audit --json', returnStatus: true)
                    if (auditStatus != 0) {
                        echo "npm audit found vulnerabilities, but continuing pipeline."
                    }
                }
            }


        }
    }
}
// This is a test commit to trigger Jenkins build. ✅
