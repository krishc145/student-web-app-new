pipeline {
    agent any

    environment {
        MAVEN_HOME = "C:\\ProgramData\\Jenkins\\.jenkins\\tools\\hudson.tasks.Maven_MavenInstallation\\maven"
        JAVA_HOME = "C:\\Program Files\\Java\\jdk1.8.0_341"
        PATH = "${env.JAVA_HOME}\\bin;${env.MAVEN_HOME}\\bin;${env.PATH}"
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10')) // keep last 10 builds
        timestamps()
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                deleteDir() // deletes the workspace to free up space
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

        stage('Deploy to Tomcat') {
            steps {
                bat "${MAVEN_HOME}\\bin\\mvn.cmd tomcat7:deploy"
            }
        }
    }

    post {
        success {
            echo "Build and deployment succeeded!"
        }
        failure {
            echo "Build failed. Check the console output."
        }
    }
}
