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
                sh 'npm audit --json > audit-report.json'
                archiveArtifacts artifacts: 'audit-report.json', fingerprint: true
            }
        }
    }
}
