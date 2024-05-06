pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/apostoll13th/jenkins.git'
            }
        }

        stage('Create Tag') {
            steps {
                script {
                    sh "git tag -d v0.1 || true"
                    sh "git tag v0.1"
                }
            }
        }

        stage('Create Branch') {
            steps {
                script {
                    sh "git branch -D v0.2-rc1 || true"
                    sh "git checkout -b v0.2-rc1"
                }
            }
        }

        stage('Push Changes') {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(credentialsId: 'github-ssh-credentials', keyFileVariable: 'SSH_KEY')]) {
                        sh "git push origin --all"
                        sh "git push origin --tags"
                    }
                }
            }
        }
    }
}
