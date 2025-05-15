pipeline {
    agent any

    environment {
        // Use your Jenkins credential ID for Snyk token here
        SNYK_TOKEN = credentials('SNYK_TOKEN')
        // Redirect Snyk config to workspace to avoid permission issues
        XDG_CONFIG_HOME = "${env.WORKSPACE}/.config"
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
                // Run snyk test using the token, ignore failure so pipeline can continue
                sh 'npx snyk test --auth=$SNYK_TOKEN || true'
            }
        }
    }

    post {
        failure {
            emailext (
                to: 'sonis95190618@gmail.com',
                subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Please check the Jenkins build details: ${env.BUILD_URL}"
            )
        }
    }
}
