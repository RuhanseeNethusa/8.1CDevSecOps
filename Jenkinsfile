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
                        sh 'npm test'
                        emailext (
                            subject: "SUCCESS: Test Stage",
                            body: "The Test stage passed successfully",
                            to: "ruhansee@gmail.com",
                            attachLog: true
                        )
                    } catch (e) {
                        currentBuild.result = 'FAILURE'
                        emailext (
                            subject: "FAILED: Test Stage",
                            body: "The Test stage failed. Check Jenkins logs.",
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
                        sh 'npm audit'
                        emailext (
                            subject: "SUCCESS: Security Scan Stage",
                            body: "The Security Scan passed with no blocking issues",
                            to: "ruhansee@gmail.com",
                            attachLog: true
                        )
                    } catch (e) {
                        currentBuild.result = 'FAILURE'
                        emailext (
                            subject: "FAILED: Security Scan Stage",
                            body: "Security scan found issues. Review logs.",
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
                body: "The pipeline has completed with status: ${currentBuild.currentResult}",
                to: "ruhansee@gmail.com",
                attachLog: true
            )
        }
    }
}
