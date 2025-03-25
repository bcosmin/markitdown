pipeline {
    agent any
    
    environment {
        UPSTREAM_REPO = 'https://github.com/microsoft/markitdown.git'
        BRANCH = 'main' // Replace with actual branch name
    }
    
    stages {
        stage('Clone Forked Repo') {
            steps {
                script {
                    // Clone the forked repository
                    bat 'git clone %GIT_URL% repo'
                    dir('repo') {
                        bat 'git remote -v'  // Verify remotes
                    }
                }
            }
        }
        
        stage('Add and Fetch Upstream') {
            steps {
                script {
                    dir('repo') {
                        bat 'git remote add upstream %UPSTREAM_REPO% || echo Upstream already added'
                        bat 'git fetch upstream'
                    }
                }
            }
        }
        
        stage('Check Jenkinsfile Updates') {
            steps {
                script {
                    dir('repo') {
                        def changes = bat(script: 'git diff upstream/%BRANCH% -- Jenkinsfile', returnStdout: true).trim()
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
