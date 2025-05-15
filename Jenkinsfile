pipeline {
    agent any

    environment {
        SNYK_TOKEN = credentials('SNYK_TOKEN')
    }

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npx snyk test || true' // Continue even if this fails
            }
        }

        stage('Snyk Scan') {
            steps {
                withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
                    sh 'npx snyk test --json > snyk-report.json || true'
                }
            }
        }
    }

    post {
        always {
            emailext (
                to: 'sonis95190618@gmail.com',
                subject: "Jenkins Build ${currentBuild.currentResult}: ${env.JOB_NAME}",
                body: "Build result: ${currentBuild.currentResult}. Check the console output for details."
            )
        }
    }
}
