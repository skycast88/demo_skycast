pipeline {
    agent any

    environment {
        SONARQUBE_URL = 'http://localhost:9000'
        SONARQUBE = 'LocalSonarQube' // The name of the SonarQube server defined in Jenkins
        SONAR_SCANNER_HOME = '/opt/sonar-scanner/bin' // Path to your local SonarQube Scanner installation
        NODEJS = "C:\\Program Files\\nodejs;${env.PATH}"
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

        stage('Build') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'npm run build'
                    } else {
                        bat 'npm run build'
                    }
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube analysis using Docker
                    docker.image('sonarsource/sonar-scanner-cli').inside {
                        sh """
                            sonar-scanner \
                            -Dsonar.projectKey=com.skycast \
                            -Dsonar.sources=src \
                            -Dsonar.host.url=$SONARQUBE_URL \
                        """
                    }
                }
            }
        }
        
    }

}
