pipeline {
    agent any

    environment {
    NETLIFY_SITE_ID = 'b81e3e1d-f081-410e-a940-a4da898f4275'
    NETLIFY_AUTH_TOKEN = credentials('netlify-token')
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
                    npm install
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

        stage('Deploy staging'){
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                npm install -g netlify-cli
                    netlify --version
                    echo "Deploying to staging. Site ID: $NETLIFY_SITE_ID"
                    netlify status
                    netlify deploy --dir=build
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
