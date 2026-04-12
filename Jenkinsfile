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
                script {
                    if (fileExists('build/index.html')){
                        echo 'build/index.html found'
                    } else {
                        error('build/index.html not found')
                    }
                }
            }
        }
    }
}