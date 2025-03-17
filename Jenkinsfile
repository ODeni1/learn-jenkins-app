pipeline {
    agent any

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
            steps {
                sh ''' 
            echo "Test Stage"
            if test -f "build/index.html";
             then
                echo "File Exists"
             else 
                echo "File not found"
                exit 1
                fi
            '''
                npm test
            }
        }
    }
}
