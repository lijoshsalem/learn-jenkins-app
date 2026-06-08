pipeline {
    agent any

    stages {
        //This is a comment
        /*
        line 1
        line 2
        */
        stage('Build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    args '-v /var/jenkins_home/npm_cache:/npm_cache'
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
        stage('Test') {
            agent{
                docker{
                    image 'node:18-alpine'
                    args '-v /var/jenkins_home/npm_cache:/npm_cache'
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
                    echo "Test stage"
                    ls -l
                    test -f "build/index.html"
                    npm test
                '''
            }
        }
        stage('E2E Test') {
            agent{
                docker{
                    image 'mcr.microsoft.com/playwright:v1.60.0-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build &
                    sleep 10
                    npx playwright test
                '''
            }
        }
    }
    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
}