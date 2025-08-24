pipeline {
    agent any

    environment {
        EMAIL_RECIPIENT = 'krishnakumarchinnusamy@gmail.com'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/krishc145/student-web-app-new.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }

    post {
        success {
            mail to: "${EMAIL_RECIPIENT}",
                 subject: "✅ Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: """Hello,

The build for ${env.JOB_NAME} #${env.BUILD_NUMBER} has completed successfully.

Regards,
Jenkins"""
        }

        failure {
            mail to: "${EMAIL_RECIPIENT}",
                 subject: "❌ Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: """Hello,

The build for ${env.JOB_NAME} #${env.BUILD_NUMBER} has failed.

Regards,
Jenkins"""
        }
    }
}
