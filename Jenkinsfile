pipeline {
  agent any

  environment {
    // Replace with your actual Jenkins credential ID (case-sensitive)
    SONAR_TOKEN = credentials('SONAR_TOKEN')  // ⬅️ Use exact ID configured in Jenkins
  }

  parameters {
    string(name: 'BRANCH_NAME', defaultValue: 'thanu.developer', description: 'Git branch to build')
  }

  triggers {
    githubPush()
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
        withSonarQubeEnv('MySonar') { // Must match name in Configure System > SonarQube servers
          sh '''
            export PATH=$PATH:/var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/sonar-scanner/bin
            sonar-scanner \
              -Dsonar.projectKey=myproject \
              -Dsonar.sources=. \
              -Dsonar.login=$SONAR_TOKEN
          '''
        }
