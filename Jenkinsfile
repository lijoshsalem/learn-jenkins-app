pipeline {
    agent any

    stages {
        stage('Build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    npm config set prefix ~/.npm-global
                    export PATH=$HOME/.npm-global/bin:$PATH
                    node --version
                    npm --version0
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
    }
}