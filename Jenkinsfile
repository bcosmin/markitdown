pipeline {
    agent {label 'dev'}
    
    environment {
        UPSTREAM_REPO = 'https://github.com/microsoft/markitdown.git'
        BRANCH = 'main'
    }
    
    stages {
        stage('Clone Forked Repo') {
            steps {
                script {
                    sh 'git clone $GIT_URL repo'
                    dir('repo') {
                        sh 'git remote -v'
                    }
                }
            }
        }
        
        stage('Add and Fetch Upstream') {
            steps {
                script {
                    dir('repo') {
                        sh 'git remote add upstream $UPSTREAM_REPO || true'
                        sh 'git fetch upstream'
                    }
                }
            }
        }
        
        stage('Check Jenkinsfile Updates') {
            steps {
                script {
                    dir('repo') {
                        def changes = sh(script: 'git diff upstream/$BRANCH -- Jenkinsfile', returnStdout: true).trim()
                        if (changes) {
                            echo "Jenkinsfile has updates!"
                        } else {
                            echo "Jenkinsfile is up to date."
                        }
                    }
                }
            }
        }
    }
}
