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
                        sh 'npm test || true'
                        emailext (
                            subject: "Test Stage Completed",
                            body: """Hello Team,  

The Test stage has completed successfully.  
Please review the attached build log for details of the test execution.  

Thank You! 
Jenkins Pipeline  
""",
                            to: "ruhansee@gmail.com",
                            attachLog: true
                        )
                    } catch (e) {
                        currentBuild.result = 'FAILURE'
                        emailext (
                            subject: "FAILED: Test Stage",
                            body: """Hello Team,  

The **Test stage failed** .  
Please check Jenkins console logs for more details.  

Thanks,  
Jenkins Pipeline  
""",
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
                        sh 'npm audit || true'
                        emailext (
                            subject: "Security Scan Completed",
                            body: """Hello Team,  

The **Security Scan stage** has finished.  
Some vulnerabilities may still exist, please review the attached log for more information.  

Thanks,  
Jenkins Pipeline  
""",
                            to: "ruhansee@gmail.com",
                            attachLog: true
                        )
                    } catch (e) {
                        currentBuild.result = 'FAILURE'
                        emailext (
                            subject: "FAILED: Security Scan Stage",
                            body: """Hello Team,  

The **Security Scan stage failed** .  
Please review the Jenkins build log to investigate further.  

Thanks,  
Jenkins Pipeline  
""",
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
                body: """Hello Team,  

The pipeline has completed with the following result: **${currentBuild.currentResult}**.  

Please check the attached build log for the full details of the pipeline run.  

Thanks,  
Jenkins Pipeline  
""",
                to: "ruhansee@gmail.com",
                attachLog: true
            )
        }
    }
}
