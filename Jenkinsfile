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
            emailext subject: "Jenkins Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "The job ${env.JOB_NAME} build #${env.BUILD_NUMBER} has completed successfully. A new JAR has been generated.",
                to: 'your_email@domain.com'
        }
        failure {
            emailext subject: "Jenkins Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "The job ${env.JOB_NAME} build #${env.BUILD_NUMBER} has failed.",
                to: 'your_email@domain.com'
        }
    }
}
