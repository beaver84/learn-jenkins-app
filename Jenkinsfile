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
        stage('Test'){
            steps {
                sh '''
                    if [ -f build/index.html ]; then
                        echo "build/index.html exists"
                    echo
                        echo "build/index.html not found"
                        exit 1
                    fi
                '''
            }
        }
    }
}