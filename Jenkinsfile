pipeline {
    agent any

    environment {
        SNYK_TOKEN = credentials('SNYK_TOKEN')
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Snyk Scan') {
            steps {
                sh 'npx snyk test --auth=$SNYK_TOKEN || true'
            }
        }
    }

    post {
        failure {
            emailext to: 'sonis95190618@gmail.com',
                     subject: "Build Failed: ${currentBuild.fullDisplayName}",
                     body: "Check Jenkins for details: ${env.BUILD_URL}"
        }
    }
}
