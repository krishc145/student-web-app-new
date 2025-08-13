pipeline {
    agent any

    tools {
        maven 'Maven3' // Name of Maven installation in Jenkins
        jdk 'JDK11'    // Name of JDK installation in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/krishc145/student-web-app-new', branch: 'krishna'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sh 'mvn tomcat7:redeploy'
            }
        }
    }

    post {
        success {
            emailext(
                subject: "Jenkins Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "The job ${env.JOB_NAME} build #${env.BUILD_NUMBER} has completed successfully. A new WAR has been generated and deployed to Tomcat.",
                to: 'krishnakumarchinnusamy@gmail.com'
            )
        }
        failure {
            emailext(
                subject: "Jenkins Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "The job ${env.JOB_NAME} build #${env.BUILD_NUMBER} has failed.",
                to: 'krishnakumarchinnusamy@gmail.com'
            )
        }
    }
}
