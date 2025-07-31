pipeline {
    agent any
     
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Thanushree841/pipeline.git'
            }
        }

        stage('Print Parameters') {
            steps {
                echo "Running in environment: ${params.ENV}"
                echo "Action selected: ${params.ACTION}"
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SonarQube') {
                        script {
                            // Get Sonar Scanner path
                            def scannerHome = tool 'sonar_scanner'  // this name must match the one in Global Tool Config
                            sh """
                              ${scannerHome}/bin/sonar-scanner \
                                -Dsonar.projectKey=myproject \
                                -Dsonar.sources=. \
                                -Dsonar.host.url=http://13.201.65.236:9000 \
                                -Dsonar.login=$SONAR_TOKEN
                            """
                        }
                    }
                }
            }
        }
    }
}
