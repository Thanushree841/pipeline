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
                echo "Checked out branch: ${params.BRANCH_NAME}"
            }
        }

        stage('Build') {
            steps {
                echo "Building the project with Maven..."
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "Starting SonarQube analysis..."
                withSonarQubeEnv('MySonar') {  // Name should match Jenkins → Configure System → SonarQube
                    sh "mvn sonar:sonar -Dsonar.token=$SONAR_TOKEN"
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline succeeded.'
            
            // Archive built JAR files
            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            
            // Publish test results if any
            junit 'target/surefire-reports/*.xml'
        }

        failure {
            echo '❌ Pipeline failed. Please check the build logs.'
        }

        always {
            echo 'ℹ️ Pipeline completed.'
        }
    }
}
