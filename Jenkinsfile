pipeline {
    agent any

    environment {
        // Set JAVA_HOME to your installed JDK (system-wide path is preferred)
        JAVA_HOME = 'C:\\Users\\krish\\AppData\\Local\\Programs\\Eclipse Adoptium\\jdk-17.0.16.8-hotspot'
        PATH = "${JAVA_HOME}\\bin;${env.PATH}"

        // Email recipient
        EMAIL_RECIPIENT = 'krishnakumarchinnusamy@gmail.com'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub
                git branch: 'main', url: 'https://github.com/krishc145/student-web-app-new.git'
            }
        }

        stage('Build') {
            steps {
                // Windows agent, use bat instead of sh
                bat 'mvn clean package'
            }
        }
    }

    post {
        success {
            mail to: "${EMAIL_RECIPIENT}",
                 subject: "✅ Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: """Hello,

The Jenkins job ${env.JOB_NAME} build #${env.BUILD_NUMBER} has completed successfully.
A new JAR has been generated in the target folder.

Regards,
Jenkins"""
        }

        failure {
            mail to: "${EMAIL_RECIPIENT}",
                 subject: "❌ Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: """Hello,

The Jenkins job ${env.JOB_NAME} build #${env.BUILD_NUMBER} has failed.

Please check the console output for details.

Regards,
Jenkins"""
        }
    }
}
