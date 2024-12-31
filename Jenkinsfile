pipeline {
    agent any
    tools {
        nodejs 'nodejs-20.11.0'
    }

    environment {
        NODEJS_HOME = '/usr/local/bin/node'  // Update this path if Node.js is installed elsewhere
        SONAR_SCANNER_PATH = '/Users/prabh/Downloads/sonar-scanner-6.2.1.4610-macosx-x64/bin'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                // Set the PATH and install dependencies using npm
                sh '''
                export PATH=$NODEJS_HOME:$PATH
                npm install
                '''
            }
        }

        stage('Lint') {
            steps {
                // Run linting to ensure code quality
                sh '''
                export PATH=$NODEJS_HOME:$PATH
                npm run lint
                '''
            }
        }

        stage('Build') {
            steps {
                // Build the React app
                sh '''
                export PATH=$NODEJS_HOME:$PATH
                npm run build
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Run sonar-scanner with the specified parameters
                sh '''
                export PATH=$SONAR_SCANNER_PATH:$PATH
                sonar-scanner \
                  -Dsonar.projectKey=proj21 \
                  -Dsonar.sources=. \
                  -Dsonar.host.url=http://localhost:9000 \
                  -Dsonar.token=sqp_75a2b2c92b8d9885b6dcd8d1e8b8fe1d7f248d11
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            echo 'This runs regardless of the result.'
        }
    }
}
