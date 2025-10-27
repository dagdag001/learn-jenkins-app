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
        stage('Build') {
  steps {
    script {
      docker.image('node:18-alpine').inside {
        sh 'npm run build'
      }
    }
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
                    image 'mcr.microsoft.com/playwright:v1.56.1-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                npm i serve 
                node_modules/.bin/serve -s build &
                sleep 10
                npx playwright test --reporter=html
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
