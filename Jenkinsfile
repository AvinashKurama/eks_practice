import hudson.FilePath

pipeline {
    agent any
    tools {
        maven "mvn"
    }

    stages {
        stage("Build Maven") {
            steps {
                script {
                    // Set up Maven tool
                    def mvnHome = tool name: 'MAVEN', type: 'hudson.tasks.Maven$MavenInstallation'
                    def mavenHome = tool name: 'MAVEN', type: 'hudson.tasks.Maven$MavenInstallation'
                    def mavenSettings = new FilePath(mvnHome).child('conf/settings.xml')
                    
                    // Checkout the code and build
                    checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GIT_REPO', url: 'https://github.com/AvinashKurama/eks_practice.git']]])
                    sh "${mavenHome}/bin/mvn -s ${mavenSettings} -Dmaven.test.failure.ignore=true clean package"
                }
            }
        }

        environment {
            NEXUS_VERSION = "nexus3"
            NEXUS_PROTOCOL = "http"
            NEXUS_URL = "54.173.247.114:8081"
            NEXUS_REPOSITORY = "maven-central-repo"
            NEXUS_CREDENTIAL_ID = "NEXUS_REPO"
        }

        stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                    def pom = readMavenPom file: "pom.xml"
                    def filesByGlob = findFiles(glob: "target/*.${pom.packaging}")
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    def artifactPath = filesByGlob[0].path
                    def artifactExists = fileExists artifactPath
                    if (artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}"
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: "${pom.groupId}",
                            version: "${pom.version}",
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                [
                                    artifactId: 'pom.docker-spring-boot',
                                    classifier: '',
                                    file: artifactPath,
                                    type: pom.packaging
                                ],
                                [
                                    artifactId: 'pom.docker-spring-boot',
                                    classifier: '',
                                    file: "pom.xml",
                                    type: "pom"
                                ]
                            ]
                        )
                    } else {
                        error "*** File: ${artifactPath}, could not be found"
                    }
                }
            }
        }
    }
}
