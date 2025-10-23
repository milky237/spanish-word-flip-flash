pipeline {
    agent any
    
    options {
        ansiColor('xterm')
    }

    stages {
        stage('build') {
            agent {
                docker {
                    image 'node:22-alpine'
                }
            }
            steps {
                sh 'npm ci'
                sh 'npm run build'
            }
        }

        stage('test') {
            parallel {
                stage('unit tests') {
                    agent {
                        docker {
                            image 'node:22-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        // Unit tests with Vitest
                        sh 'npm ci'
                        sh 'npx vitest run --reporter=verbose'
                    }
                }
                // stage('integration tests') {
                //     agent {
                //         docker {
                //             image 'mcr.microsoft.com/playwright:v1.56.1-jammy'
                //             reuseNode true
                //         }
                //     }
                //     steps {
                //         sh 'npx playwright test'
                //     }
                // }
            }
        }

        stage('deploy') {
            agent {
                docker {
                    image 'alpine'
                }
            }
            steps {
                // Mock deployment which does nothing
                echo 'Mock deployment was successful!'
            }
        }

        stage('e2e') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.56.1-jammy'
                    reuseNode true
                }
            }
            environment {
                E2E_BASE_URL = 'https://spanish-cards.netlify.app/'
            }
            steps {
                sh 'npx playwright test'
            }
            post {
                always {
                    publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, icon: '', keepAll: false, reportDir: 'reports-e2e/html/', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                    junit stdioRetention: 'ALL', testResults: 'reports-e2e/junit.xml'
                }
            }
        }
    }
}