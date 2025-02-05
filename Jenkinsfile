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
                            url: 'https://github.com/skycast88/demo_skycast.git',    
                            credentialsId: '02fcd2c0-a404-4be0-8216-2e51edf0b945' // Replace with your Jenkins credentials ID
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
        
    }

}
