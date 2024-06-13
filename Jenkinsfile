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
            when {
                expression {
                    def latestTag = sh(returnStdout: true, script: 'git describe --tags --abbrev=0').trim()
                    return tag != latestTag
                }
            }
            steps {
                sh "mkdir -p build-${tag}"
                sh "cp index.html build-${tag}/"
            }
        }
        
        stage('Deploy') {
            when {
                expression {
                    def latestTag = sh(returnStdout: true, script: 'git describe --tags --abbrev=0').trim()
                    return tag != latestTag
                }
            }
            steps {
                sh "ln -sfn build-${tag} public_html"
            }
        }
    }
}
