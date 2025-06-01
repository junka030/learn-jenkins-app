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
        
        stage('Test') {
            steps {
                sh '''
                    test -f build/index.html 
                    npm test
                '''
            }
        }
    }
}
