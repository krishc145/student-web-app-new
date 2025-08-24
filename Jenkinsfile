pipeline {
    agent any

    environment {
        JAVA_HOME = 'C:\\Java\\jdk-17.0.12'
        PATH = "${JAVA_HOME}\\bin;${env.PATH}"
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
                bat 'mvn clean package'
            }
        }
    }

    post {
        success {
            mail to: "${EMAIL_RECIPIENT}",
                 subject: "✅ Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Build completed successfully!"
        }

        failure {
            mail to: "${EMAIL_RECIPIENT}",
                 subject: "❌ Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                 body: "Build failed!"
        }
    }
}
