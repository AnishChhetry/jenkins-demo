pipeline {
    agent any

    // Requesting the NodeJS tool configured globally
    tools {
        nodejs 'node10'
    }

    // Defining parameters for manual trigger overrides
    parameters {
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Check to execute tests')
        choice(name: 'DEPLOY_ENV', choices: ['staging', 'production'], description: 'Select deployment environment')
    }

    // Defining environment variables and securely binding Jenkins credentials
    environment {
        APP_VERSION = '1.0.0'
        SERVER_CREDS = credentials('deploy-server-creds')
    }

    stages {
        stage('Build') {
            steps {
                echo "Building application version: ${APP_VERSION}"
                // Simulate NodeJS build
                sh 'npm --version' 
            }
        }

        stage('Test') {
            // Conditional execution based on UI parameter AND branch name
            when {
                expression { return params.RUN_TESTS }
                branch 'dev'
            }
            steps {
                echo "Running tests on the dev branch..."
            }
        }

        stage('Deploy') {
            // Conditional execution: only deploy from the master branch
            when {
                branch 'master'
            }
            steps {
                echo "Deploying version ${APP_VERSION} to ${params.DEPLOY_ENV}..."
                // Using the securely bound credentials (do not echo passwords in real projects!)
                echo "Connecting to server using user: ${SERVER_CREDS_USR}"
            }
        }
    }

    // Post actions run after all stages
    post {
        always {
            echo "Pipeline execution is complete."
        }
        success {
            echo "Build succeeded! Sending notification..."
        }
        failure {
            echo "Build failed. Check the logs."
        }
    }
}
