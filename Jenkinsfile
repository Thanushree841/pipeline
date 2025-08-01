pipeline {
    agent any

    tools {
        maven 'Maven 3.9.4'                 // Must match the tool name in Jenkins → Global Tool Configuration
        sonarQubeScanner 'sonar-scanner'    // Must match the name you configured in Jenkins tools
    }

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN')  // Jenkins secret text credentials ID
    }

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Git branch to build')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('MySonar') {
                    sh "mvn sonar:sonar -Dsonar.token=$SONAR_TOKEN"
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
        success {
            echo 'Pipeline succeeded.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
