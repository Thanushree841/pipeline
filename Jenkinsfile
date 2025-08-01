pipeline {
  agent any

  tools {
    // Reference the tool name as defined above
    sonarScanner 'sonar_scanner'
  }

  environment {
    SONAR_TOKEN = credentials(' SONAR_TOKEN') // Jenkins credential ID
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
        withSonarQubeEnv('MySonar') {
          sh '''
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
        timeout(time: 5, unit: 'MINUTES') {
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
