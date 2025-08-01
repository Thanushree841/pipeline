pipeline {
    agent any
    }

    environment {
        // This must match the name configured in Global Tool Configuration
        SONAR_SCANNER_HOME = tool 'sonar_scanner'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'thanu.developer', url: 'https://github.com/Thanushree841/pipeline.git'
            }
        }

        stage('Print Parameters') {
            steps {
                echo "Running in environment: ${params.ENV}"
                echo "Action selected: ${params.ACTION}"
            }
        }

        stage('SonarQube Scan') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('MySonar') {
                        echo "Running Sonar Scanner from: ${SONAR_SCANNER_HOME}/bin/sonar-scanner"
                        sh "${SONAR_SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectKey=myproject -Dsonar.sources=. -Dsonar.host.url=http://13.203.103.245:30900 -Dsonar.login=${SONAR_TOKEN}"
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build & Package') {
            steps {
                echo "Build and packaging logic would go here."
                // Example (Uncomment if using Maven):
                // sh 'mvn clean package'
                // archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }
}
