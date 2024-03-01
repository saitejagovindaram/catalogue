pipeline {
    // agent {
    //     node {
    //         label 'agent-1'
    //     }
    // }
    agent any

    environment {
        packageVersion = ''
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
    }
    post {
        always{
            echo 'I will run always'
            // deleteDir()
        }
    }
}