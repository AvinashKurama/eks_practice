pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        mvn "M3"
    }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/AvinashKurama/eks_practice.git'
                sh "cd spring-boot-docker"
                // Run Maven on a Unix agent.
                sh "mvn  clean package"
            }

        }
    }
}
