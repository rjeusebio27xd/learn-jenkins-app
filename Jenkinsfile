pipeline {
    agent any

    stages {
        /*
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
                    npm -- version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        */
        stage('Test'){
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                echo "Testing Stage"
                #test -f build/index.html
                npm test
                '''
            }
        } 
   
    stage('Cleanup') {
        steps {
            deleteDir()   // Jenkins native cleanup, works without rm
        }
    }
    

                stage('E2E'){
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.55.0-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                 npm install serve
                node_modules/.bin/serve -s build &
                sleep 10
                rm -rf test-results/
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

