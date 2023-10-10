pipeline {
    agent any
    tools {
        maven "mvn"
    }


    stages {
        stage("Build Maven") {
            steps {
                script {
                    // Checkout the code and build
                    checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/AvinashKurama/eks_practice.git']]])
                    sh "${mavenHome}/bin/mvn -s ${mavenSettings} -Dmaven.test.failure.ignore=true clean package"
                }
            }
        }
    }
}

