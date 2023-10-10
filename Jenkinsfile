pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/AvinashKurama/eks_practice.git'
                sh "cd spring-boot-docker"
                sh "sudo apt install maven -y"
                // Run Maven on a Unix agent.
                sh "mvn  clean package"
            }

        }
    }
}
