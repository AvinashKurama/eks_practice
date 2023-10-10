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
        
     stage("Publish to Nexus Repository Manager") {
        steps {
            script{
            def mavenPom = readMavenPom file: 'pom.xml'
            nexusArtifactUploader artifacts: [
                [
                    artifactId: 'docker-spring-boot', 
                    classifier: '', 
                    file: "target/docker-spring-boot-${mavenPom.version}.jar", 
                    type: 'jar'
                    ]
                ], 
            credentialsId: NEXUS_CREDENTIAL_ID,
            groupId: 'com.mkyong', 
            nexusUrl: NEXUS_URL, 
            nexusVersion: NEXUS_VERSION, 
            protocol: NEXUS_PROTOCOL, 
            repository: NEXUS_REPOSITORY, 
            version: "${mavenPom.version}"
            }
            
        }
     }
    }
}