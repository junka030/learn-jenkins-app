pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'e884a645-a01e-43af-9151-63790af41ce7'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                 }
            }

            steps {
                // sh 'echo "Hello warudo 0 v 0"'
                sh '''
                    ls -la
                    node  --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        
        stage('Tests') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    test -f build/index.html 
                    npm test
                '''
            }   

            post {
                always {
                    junit 'test-results/junit.xml'
                }
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                 }
            }

            steps {
                sh '''
                    npm install netlify-cli 
                    node_modules/.bin/netlify --version
                    echo "Deploying to Netlify with site ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                '''
            }
        } 
    }
}
