pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/AvinashKurama/eks_practice.git'
                sh "ls"
                sh "cd spring-boot-docker"
                // Run Maven on a Unix agent.
                sh "mvn  clean package"
            }

        }
    }
}
