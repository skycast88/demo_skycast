pipeline {
    agent any

    environment {
        SONARQUBE = 'Sonarqube' // The name of the SonarQube server defined in Jenkins
        SONAR_SCANNER_HOME = 'C:\\sonar-scanner' // Path to your local SonarQube Scanner installation
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
                    // Run the SonarQube analysis
                    withSonarQubeEnv(SONARQUBE) {
                        if (isUnix()) {
                            sh '''sonar-scanner \
                                -Dsonar.projectKey=com.skycast \
                                -Dsonar.projectName="Skycast" \
                                -Dsonar.sources=src'''
                        } else {
                            bat '''sonar-scanner \
                                -Dsonar.projectKey=com.skycast \
                                -Dsonar.projectName="Skycast" \
                                -Dsonar.sources=src'''
                        }
                    }
                }
            }
        }
        
    }

}
