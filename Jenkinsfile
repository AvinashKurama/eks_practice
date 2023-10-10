pipeline {
    agent any
    tools {
        maven "mvn"
    }


     stages {
        stage("Clone code from GitHub") {
            steps {
                script {
                    git branch: 'main', url: 'https://github.com/AvinashKurama/eks_practice.git';
                }
            }
        }
        stage("Maven Build") {
            steps {
                script {
                    sh "mvn package -DskipTests=true"
                }
            }
        }
    }
}

