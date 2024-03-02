pipeline {
    agent {
        node {
            label 'agent-1'
        }
    }
    // agent any

    environment {
        packageVersion = ''
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "34.229.220.6:8081"
        NEXUS_REPOSITORY = "catalogue"
        NEXUS_CREDENTIAL_ID = 'nexus-auth'
    }

    options {
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    parameters {
        string(name: 'version', defaultValue: '', description: 'what is the version?')
        string(name: 'environment', defaultValue: '', description: 'what is the environment?')
    }
    stages {
        stage('Get the version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    packageVersion = packageJson.version
                    echo "application version: ${packageVersion}"
                }
            }
        }
        stage('install dependencies') {
            steps {
                sh '''
                    npm install
                '''
            }
        }
        stage('Build'){
            steps {
                sh '''
                    ls -ltr
                    zip -q -r catalogue.zip ./*
                    ls -ltr
                '''
            }
        }

        stage('Publish artifact'){
            steps {
                script {
                    nexusArtifactUploader(
                            nexusVersion: "${NEXUS_VERSION}",
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: 'com.roboshop',
                            version: "${packageVersion}",
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                // Artifact generated such as .jar, .ear and .war files.
                                [artifactId: 'catalogue',
                                classifier: '',
                                file: 'catalogue.zip',
                                type: 'zip'],

                                // // Lets upload the pom.xml file for additional information for Transitive dependencies
                                // [artifactId: pom.artifactId,
                                // classifier: '',
                                // file: "pom.xml",
                                // type: "pom"]
                            ]
                        );
                }
            }
        }
    }
    post {
        always{
            echo 'I will run always'
            deleteDir()
        }
    }
}