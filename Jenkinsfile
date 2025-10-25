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
                    set -eux
                    echo "Running as $(whoami)"
                    mkdir -p $PWD/.npm-global $PWD/.npm
                    export NPM_CONFIG_USERCONFIG=$PWD/.npmrc
                    npm config set prefix $PWD/.npm-global
                    npm config set cache $PWD/.npm
                    export PATH=$PWD/.npm-global/bin:$PATH
                    npm ci --cache $PWD/.npm
                    npm run build
                    ls -la
                '''
            }
        }
    }
}