pipeline {
    agent any

    environment {
        NODE_ENV = 'test' // Set Node.js environment to 'test'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout code from GitLab repository
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: 'main']],
                        userRemoteConfigs: [[
                            url: 'https://gitlab-in.globallogic.com/rajani.ekunde/nodetest-day3-jenkins.git',
                            credentialsId: 'gitlab-credentials-id' // Replace with your Jenkins credentials ID
                        ]]
                    ])
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'npm install' // Install Node.js dependencies
                    } else {
                        bat 'npm install'
                    }
                }
            }
        }

        stage('Lint Code') {
            steps {
                script {
                    if (isUnix()) {
                        sh '''
                            if [ -f .eslintrc.js ] || [ -f eslint.config.js ]; then
                                npx eslint . --fix
                            else
                                echo "No ESLint configuration file found. Skipping linting."
                            fi
                        '''
                    } else {
                        bat '''
                            if exist .eslintrc.js ( 
                                npx eslint . --fix 
                            ) else (
                                echo "No ESLint configuration file found. Skipping linting."
                            )
                        '''
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'npm test' // Run tests
                    } else {
                        bat 'npm test'
                    }
                }
            }
        }

        
    }

    post {
        always {
            cleanWs() // Clean up the workspace after the pipeline run
        }
        success {
            echo 'Build and tests were successful!'
        }
        failure {
            echo 'Build or tests failed. Please check the logs.'
        }
    }
}
