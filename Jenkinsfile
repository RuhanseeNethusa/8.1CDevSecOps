pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/RuhanseeNethusa/8.1CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    try {
                        sh 'npm test || true'   // allows pipeline to continue even if tests fail
                        emailext (
                            subject: "Test Stage Completed",
                            body: "The Test stage executed (may include failures). Check logs for details.",
                            to: "ruhansee@gmail.com",
                            attachLog: true
                        )
                    } catch (e) {
                        currentBuild.result = 'FAILURE'
                        emailext (
                            subject: "FAILED: Test Stage",
                            body: "The Test stage crashed. Please check logs.",
                            to: "ruhansee@gmail.com",
                            attachLog: true
                        )
                        throw e
                    }
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    try {
                        sh 'npm audit || true'   //  also continues even if vulnerabilities found
                        emailext (
                            subject: "Security Scan Completed",
                            body: "The Security Scan executed (may include vulnerabilities). Check logs.",
                            to: "ruhansee@gmail.com",
                            attachLog: true
                        )
                    } catch (e) {
                        currentBuild.result = 'FAILURE'
                        emailext (
                            subject: "FAILED: Security Scan Stage",
                            body: "The Security Scan crashed. Please check logs.",
                            to: "ruhansee@gmail.com",
                            attachLog: true
                        )
                        throw e
                    }
                }
            }
        }
    }

    post {
        always {
            emailext (
                subject: "Pipeline Finished - ${currentBuild.currentResult}",
                body: "The pipeline finished with status: ${currentBuild.currentResult}",
                to: "ruhansee@gmail.com",
                attachLog: true
            )
        }
    }
}
