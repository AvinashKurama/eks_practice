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
                script {
                    def pom = readMavenPom file: 'pom.xml';
                    def pomContent = readFile('pom.xml');
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: 'pom.com.mkyong',
                            version: 'pom.1.0',
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                [artifactId: 'pom.docker-spring-boot',
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                [artifactId: 'pom.docker-spring-boot',
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
        }
    }
}

