pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Prepare') {
            steps {
                script {
                    tag = sh(returnStdout: true, script: 'git describe --tags --always').trim()
                }
            }
        }
        
        stage('Build') {
            steps {
                sh "mkdir -p build-${tag}"
                sh "cp index.html build-${tag}/"
            }
        }
        
        stage('Deploy') {
            steps {
                sh "ln -sfn build-${tag} public_html"
            }
        }
    }
}
