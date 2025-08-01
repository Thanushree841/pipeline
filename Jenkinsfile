pipeline {
agent any

tools {
// Must match name in Jenkins > Global Tool Configuration > SonarQube Scanner installations
sonarQube 'sonar_scanner'
}

environment {
// Must match ID of a secret text credential in Jenkins
SONAR_TOKEN = credentials('sonar-token')
}

triggers {
githubPush() // Automatically trigger on GitHub push
}

stages {
  stage('Checkout Code') {
  steps {
    checkout scm // Checks out current branch from GitHub
  }
}

stage('SonarQube Scan') {
  steps {
    withSonarQubeEnv('MySonar') { // Must match name defined under "Configure System"
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
