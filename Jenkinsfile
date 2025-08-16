@Library("shared-library-gmail") _   // 👈 import your shared library

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from Git SCM
                git url: 'https://github.com/krishc145/student-web-app-new', branch: 'krishna'
            }
        }
        stage('Build') {
            steps {
                // Run Maven to build JAR
                sh 'mvn clean package'
            }
        }
    }

    post {
        success {
            script {
                withCredentials([string(credentialsId: 'gmail-app-password-id', variable: 'GMAIL_PASS')]) {
                    sendGmail(
                        to: "krishnakumarchinnusamy@gmail.com",
                        subject: "✅ Jenkins Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: """Hello,

The job ${env.JOB_NAME} build #${env.BUILD_NUMBER} has completed successfully. 
A new JAR has been generated.

Regards,
Jenkins
""",
                        user: "krishnakumarchinnusamy@gmail.com",
                        password: "${GMAIL_PASS}"
                    )
                }
            }
        }
        failure {
            script {
                withCredentials([string(credentialsId: 'gmail-app-password-id', variable: 'GMAIL_PASS')]) {
                    sendGmail(
                        to: "krishnakumarchinnusamy@gmail.com",
                        subject: "❌ Jenkins Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                        body: """Hello,

The job ${env.JOB_NAME} build #${env.BUILD_NUMBER} has failed.

Regards,
Jenkins
""",
                        user: "krishnakumarchinnusamy@gmail.com",
                        password: "${GMAIL_PASS}"
                    )
                }
            }
        }
    }
}
