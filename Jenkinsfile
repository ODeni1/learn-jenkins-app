pipeline {
    agent any

    environment {
    NETLIFY_SITE_ID = 'b81e3e1d-f081-410e-a940-a4da898f4275'
    }
    
    stages {
        stage('Build') {
            agent {
                docker 
                {
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
        stage('Test') {
            agent {
                docker 
                {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh ''' 
            echo "Test Stage"
            test -f "build/index.html"
            npm test
            '''
                
            }
        }

        stage('Deploy'){
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                npm install netlify-cli
                node_modules/.bin/netlify --version
                echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
                '''
            }
        }
        
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }

}
