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
                    commit = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
                }
            }
        }
        
        stage('Build') {
            when {
                expression {
                    def latestCommit = sh(returnStdout: true, script: 'git rev-parse HEAD~1').trim()
                    return commit != latestCommit
                }
            }
            steps {
                sh "mkdir -p build-${commit}"
                sh "cp index.html build-${commit}/"
            }
        }
        
        stage('Deploy') {
            when {
                expression {
                    def latestCommit = sh(returnStdout: true, script: 'git rev-parse HEAD~1').trim()
                    return commit != latestCommit
                }
            }
            steps {
                sh "ln -sfn build-${commit} public_html"
            }
        }
    }
}
