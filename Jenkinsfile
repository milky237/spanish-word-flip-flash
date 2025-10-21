pipeline {
    agent {
        docker {
            image 'node:24-alpine'
        }
    }
    
    options {
        ansiColor('xterm')
    }

    stages {
        stage('build') {
            steps {
                sh 'npm ci'
                sh 'npm run build'
            }
        }

        stage('test') {
            parallel {
                stage('unit tests') {
                    steps {
                        sh 'npx vitest run --reporter=verbose'
                    }
                }
            }
        }

        stage('deploy') {
            agent {
                docker {
                    image 'alpine'
                }
            }
            steps {
                echo 'Mock deployment was successful!'
            }
        }
    }
}
