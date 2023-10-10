pipeline {
    agent any
    tools {
        maven "mvn"
    }

    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "54.173.247.114:8081"
        NEXUS_REPOSITORY = "maven-central-repo"
        NEXUS_CREDENTIAL_ID = "NEXUS_REPO"
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
        stage('Publish to Nexus') {
    steps {
        script {
            def artifactPath = sh(script: 'find target/ -name "*.jar" | head -n 1', returnStatus: true).trim()
            def pomPath = 'pom.xml'

            // Check if the artifact and POM file exist
            if (fileExists(artifactPath) && fileExists(pomPath)) {
                nexusArtifactUploader(
                    nexusVersion: NEXUS_VERSION,
                    protocol: NEXUS_PROTOCOL,
                    nexusUrl: NEXUS_URL,
                    groupId: 'com.mkyong', // Customize your artifact coordinates
                    version: '1.0', // Customize your artifact version
                    repository: NEXUS_REPOSITORY,
                    credentialsId: NEXUS_CREDENTIAL_ID,
                    artifacts: [
                        [
                            artifactId: 'docker-spring-boot', // Replace with your artifactId
                            classifier: '',
                            file: artifactPath,
                            type: 'jar'
                        ],
                        [
                            artifactId: 'docker-spring-boot', // Replace with your artifactId
                            classifier: '',
                            file: pomPath,
                            type: 'pom'
                        ]
                    ]
                )
            } else {
                error 'Artifact or POM file not found'
            }
        }
    }
}
     }
}