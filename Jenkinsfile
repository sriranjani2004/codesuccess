pipeline {
    agent any

    tools {
        nodejs 'NodeJS' // Ensure NodeJS is installed and configured
    }

    environment {
        NODEJS_HOME = '/usr/local/bin/node'
        PATH = "$NODEJS_HOME:$PATH:/usr/local/sonar-scanner/bin"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install and Build') {
            steps {
                sh '''
                    npm install
                '''
            }
        }

        stage('SonarCodeAnalysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token')
            }
            steps {
                // Invoke sonar-scanner directly (path included in PATH environment variable)
                sh '''
                    sonar-scanner \
                    -Dsonar.projectKey=newtoken \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://localhost:9000
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline SUCCESSFULLY Build"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}
