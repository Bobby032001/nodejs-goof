pipeline {
    agent any

    environment {
        SNYK_TOKEN = credentials('SNYK_TOKEN')
        XDG_CONFIG_HOME = "${env.WORKSPACE}/.config"
        RECIPIENTS = 'sonis95190618@gmail.com'  // Change this to your desired email
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
                // Run tests and save logs to a file
                sh 'npm test | tee test.log'
            }
            post {
                always {
                    script {
                        def status = currentBuild.currentResult
                        emailext (
                            to: env.RECIPIENTS,
                            subject: "Run Tests Stage: ${status} - Job ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                            body: "The 'Run Tests' stage finished with status: ${status}\n\nCheck console output here: ${env.BUILD_URL}console",
                            attachmentsPattern: 'test.log'
                        )
                    }
                }
            }
        }

        stage('Snyk Scan') {
            steps {
                // Run snyk test and save logs to a file
                sh 'npx snyk test --auth=$SNYK_TOKEN | tee snyk.log || true'
            }
            post {
                always {
                    script {
                        def status = currentBuild.currentResult
                        emailext (
                            to: env.RECIPIENTS,
                            subject: "Snyk Scan Stage: ${status} - Job ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                            body: "The 'Snyk Scan' stage finished with status: ${status}\n\nCheck console output here: ${env.BUILD_URL}console",
                            attachmentsPattern: 'snyk.log'
                        )
                    }
                }
            }
        }
    }
}
