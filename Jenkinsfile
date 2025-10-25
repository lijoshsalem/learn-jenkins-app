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
                    node --version
                    npm --version
                    mkdir -p $PWD/.npm
                    npm config set cache $PWD/.npm --global
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
    }
}