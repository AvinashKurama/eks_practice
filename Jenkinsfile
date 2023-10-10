pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                sh '''cd spring-boot-docker
                      mvn clean package'''
            }

        }
    }
}
