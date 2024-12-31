pipeline {
    agent any
    tools {
        nodejs 'nodejs-20.11.0'
    }
 
    environment {
        NODEJS_HOME = '/usr/local/bin/node'  // Adjust if necessary based on your actual Node.js path
        SONAR_SCANNER_PATH = '/Users/ariv/Downloads/sonar-scanner-6.2.1.4610-macosx-x64/bin'  // Adjust with your actual Sonar Scanner path
        SONAR_TOKEN = 'sqp_9de12b316c2b6c1fde146c2a1b98d1ab25e27624'  // Direct SonarQube token (ensure to secure in Jenkins)
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Set the PATH and install dependencies using npm
                    sh '''
                    export PATH=$NODEJS_HOME:$PATH
                    npm install
                    '''
                }
            }
        }

        stage('Lint') {
            steps {
                script {
                    // Run linting to ensure code quality
                    sh '''
                    export PATH=$NODEJS_HOME:$PATH
                    npm run lint
                    '''
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build the React app
                    sh '''
                    export PATH=$NODEJS_HOME:$PATH
                    npm run build
                    '''
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Ensure that sonar-scanner is in the PATH and run the analysis
                    sh '''
                    export PATH=$SONAR_SCANNER_PATH:$PATH
                    if command -v sonar-scanner &>/dev/null; then
                        echo "SonarQube scanner found"
                    else
                        echo "SonarQube scanner not found. Please install it."
                        exit 1
                    fi

                    sonar-scanner \
                        -Dsonar.projectKey=success \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.token=$SONAR_TOKEN
                    '''
                }
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
