pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'd9708970-de40-4fa3-8bbf-25d57a6ca839'
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

        // stage('Test') {
        //     agent {
        //         docker {
        //             image 'node:18-alpine'
        //             reuseNode true
        //         }
        //     }

        //     steps {
        //         sh '''
        //             test -f build/index.html
        //             npm test
        //         '''
        //     }
        // }

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
                  alias netlify='node_modules/.bin/netlify' 
                  netlify --version
                  netlify status
                  netlify deploy --dir=build --prod           
                '''
            }
        }
    }
}
