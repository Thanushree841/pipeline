pipeline {
  agent any

  tools {
    sonarQube 'sonar_scanner' // Must match the name in Jenkins Global Tool Configuration
  }

  environment {
    SONAR_TOKEN = credentials('sonar-token') // Must match your Jenkins credential ID
  }

  triggers {
    githubPush() // Enables webhook trigger
  }

  stages {

    stage('Checkout Code') {
      steps {
        checkout scm
      }
    }

    stage('SonarQube Scan') {
      steps {
        withSonarQubeEnv('MySonar') { // Must match Jenkins > Configure System > SonarQube server name
          sh '''#!/bin/bash
          sonar-scanner \
            -Dsonar.projectKey=myproject \
            -Dsonar.sources=. \
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
}
