pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Test') {
            parallel {
              stage('Unit Test') {
                  steps {
                      sh '''
                          test -f build/index.html
                      '''
                  }
              }
              stage('Jest Test') {
                  agent {
                      docker {
                          image 'node:18-alpine'
                          reuseNode true
                      }
                  }
                  steps {
                      sh '''
                          npm test
                      '''
                  }
              }
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
