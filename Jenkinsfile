pipeline {
    agent {
        node {
            label 'agent-1'
        }
    }
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
    }
    stages {
        stage('install dependencies') {
            steps {
                sh '''
                    npm install
                '''
            }
        }
    }
}