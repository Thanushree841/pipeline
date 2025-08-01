pipeline {
  agent any

  tools {
    sonarQube 'sonar_scanner' // Must match name configured in Jenkins → Global Tool Configuration
  }

  environment {
    SONAR_TOKEN = credentials('sonar-token') // Replace with your actual Jenkins credential ID
  }

  triggers {
    githubPush() // Trigger build on GitHub push
  }

  stages {

    stage('Checkout Code') {
      steps {
        checkout scm
      }
    }

    stage('SonarQube Scan') {
      steps {
        withSonarQubeEnv('MySonar') { // Must match Jenkins → Configure System → SonarQube server name
          // Note: This block must be left-aligned and clean
          sh '''#!/bin/bash
sonar-scanner \\
  -Dsonar.projectKey=myproject \\
  -Dsonar.sources=. \\
  -Dsonar.login=$SONAR_TOKEN
'''
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
        sh 'mvn clean package'
        archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
      }
    }
  }

  post {
    success {
      echo '✅ Build, scan, and packaging successful.'
    }
    failure {
      echo '❌ Build or analysis failed.'
    }
  }
}
