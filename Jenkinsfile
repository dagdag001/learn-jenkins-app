pipeline {
    agent any

    stages {
         stage('Install Dependencies') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                npm ci || npm install
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                #test -f build/index.html
                npm test
                '''
            }
        }
         stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-focal'
                    reuseNode true
                }
            }
            steps {
                sh '''
                npm i serve 
                node_modules/.bin/serve -s build &
                sleep 10
                npx playwright test
                '''
            }
        }
    }
    post {
        always {
            junit 'jest-results/junit.xml'
        }
    }
}
