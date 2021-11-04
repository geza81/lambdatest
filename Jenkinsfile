#!groovy
pipeline {
    agent { label 'docker' }
    environment {
        VERSION_MAJOR = '1'
        VERSION_MINOR = '1'
        VERSION_PATCH = '0'
        VERSION = "${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_PATCH}.${BUILD_NUMBER}"       
    }
    options {
        ansiColor('xterm')
    }
    stages {
        stage('Build and Test') {
            steps {
                echo 'Writing version_information.json'
                writeFile file: 'version_information.json', text: """
                {
                    "Version": {
                        "Major": "$VERSION_MAJOR",
                        "Minor": "$VERSION_MINOR",
                        "Patch": "$VERSION_PATCH",
                        "Build": "$BUILD_NUMBER",                       
                    }
                }
                """
                echo """Building VERSION = ${VERSION}"""
                sh """aws --version"""
                sh """aws sts get-caller-identity"""
            }
        }
        stage('Publish build image') {
            steps {
                sh """aws --version"""
                sh """aws sts get-caller-identity"""
            }
        }
        stage('Publish production image') {
            when { environment name: 'GERRIT_EVENT_TYPE', value: 'change-merged' }
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    sh """aws --version"""
                    sh """aws sts get-caller-identity"""
                }
            }
        }
    }  
}
