pipeline {
  agent any

  environment {
    SONAR_TOKEN = credentials('SONAR_TOKEN')  // Jenkins credential ID for SonarQube token
  }

  parameters {
    string(name: 'BRANCH_NAME', defaultValue: 'thanu.developer', description: 'Git branch to build')
  }

  triggers {
    githubPush()
  }

  tools {                // Optional: replace with your Jenkins Maven installation name
    sonarQubeScanner 'sonar-scanner'   // Must match the name in Global Tool Configuration
  }

  stages {
    stage('Checkout Code') {
      steps {
        checkout([
          $class: 'GitSCM',
          branches: [[name: "*/${params.BRANCH_NAME}"]],
          userRemoteConfigs: [[url: 'https://github.com/Thanushree841/pipeline.git']]
        ])
      }
    }

    stage('SonarQube Scan') {
      steps {
        script {
          def scannerHome = tool name: 'sonar-scanner', type: 'hudson.plugins.sonar.SonarScannerInstallation'
          withSonarQubeEnv('MySonar') {
