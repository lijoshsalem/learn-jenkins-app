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
                    # Create writable local directories
                    mkdir -p $PWD/.npm-global
                    mkdir -p $PWD/.npm

                     # Tell npm to use them instead of root-owned paths
                     npm config set prefix $PWD/.npm-global
                     npm config set cache $PWD/.npm

                     # Add the local bin to PATH so global installs work
                     export PATH=$PWD/.npm-global/bin:$PATH

                     # Run install using safe cache directory
                     npm ci --cache $PWD/.npm
                     npm run build
                     ls -la
                '''
            }
        }
    }
}