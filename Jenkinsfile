pipeline {
    agent any

    environment {
        NODE_ENV = 'development'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/Bobby032001/nodejs-goof.git'
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
                withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
                    sh 'snyk auth $SNYK_TOKEN'
                    sh 'snyk test'
                }
            }
        }
    }

    post {
        failure {
            emailext to: 'sonis95190618@gmail.com',
                     subject: "Build Failed",
                     body: "Check Jenkins for details: ${env.BUILD_URL}"
        }
    }
}
