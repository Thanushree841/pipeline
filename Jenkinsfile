pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN')  // Use Jenkins credential ID
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
                withSonarQubeEnv('MySonar') { // 'MySonar' must match the name configured in Jenkins → Configure System
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
