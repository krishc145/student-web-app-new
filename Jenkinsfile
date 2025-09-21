pipeline {
    agent any

    environment {
        MAVEN_HOME = "C:\\ProgramData\\Jenkins\\.jenkins\\tools\\hudson.tasks.Maven_MavenInstallation\\maven"
        JAVA_HOME = "C:\\Program Files\\Java\\jdk1.8.0_341"
        PATH = "${env.JAVA_HOME}\\bin;${env.MAVEN_HOME}\\bin;${env.PATH}"
        GMAIL_USER = "krishnakumarchinnusamy@gmail.com"
        GMAIL_PASS = "ixsaxrbunuvaxluf"  // ⚠️ Move this to Jenkins credentials
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timestamps()
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout Code') {
            steps {
                git(
                    url: 'https://github.com/krishc145/student-web-app-new.git',
                    branch: 'main',
                    credentialsId: 'b0725751-8901-49f7-b432-5a0f0168ad35'
                )
            }
        }

        stage('Build & Package') {
            steps {
                bat "${MAVEN_HOME}\\bin\\mvn.cmd clean install -U"
            }
        }

        stage('Trivy Scan JAR') {
            steps {
                script {
                    // Assuming your JAR is in target/ folder
                    def jarFile = "target/student-web-app-new-1.0-SNAPSHOT.jar"

                    // Run Trivy FS scan
                    bat """
                        trivy fs --exit-code 0 --severity HIGH,CRITICAL --output trivy-report.txt ${jarFile}
                    """
                    
                    // Show report in console
                    bat "type trivy-report.txt"
                    
                    // Fail build if critical vulns found
                    bat """
                        trivy fs --exit-code 1 --severity CRITICAL ${jarFile}
                    """
                }
            }
        }
    }

    post {
        success {
            script {
                writeFile file: 'send_mail.py', text: """
import smtplib, ssl

port = 587
smtp_server = "smtp.gmail.com"
sender_email = "${GMAIL_USER}"
receiver_email = "${GMAIL_USER}"
password = "${GMAIL_PASS}"

message = f\"\"\"\
Subject: ✅ Jenkins Build SUCCESS

Hello,
Your Jenkins job for ${env.JOB_NAME} (build #${env.BUILD_NUMBER}) succeeded.

Trivy Report attached in console log.

See details: ${env.BUILD_URL}
\"\"\" 

context = ssl.create_default_context()
with smtplib.SMTP(smtp_server, port) as server:
    server.starttls(context=context)
    server.login(sender_email, password)
    server.sendmail(sender_email, receiver_email, message)
"""
                bat "python send_mail.py"
            }
        }
        failure {
            script {
                writeFile file: 'send_mail.py', text: """
import smtplib, ssl

port = 587
smtp_server = "smtp.gmail.com"
sender_email = "${GMAIL_USER}"
receiver_email = "${GMAIL_USER}"
password = "${GMAIL_PASS}"

message = f\"\"\"\
Subject: ❌ Jenkins Build FAILED

Hello,
Your Jenkins job for ${env.JOB_NAME} (build #${env.BUILD_NUMBER}) has FAILED.

Check console log for Trivy or build errors.

See details: ${env.BUILD_URL}
\"\"\" 

context = ssl.create_default_context()
with smtplib.SMTP(smtp_server, port) as server:
    server.starttls(context=context)
    server.login(sender_email, password)
    server.sendmail(sender_email, receiver_email, message)
"""
                bat "python send_mail.py"
            }
        }
    }
}
