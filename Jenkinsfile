pipeline {
  agent any

  tools {
    nodejs 'node18'
  }

  environment {
    SONAR_SCANNER_HOME = tool 'sonar-scanner'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install Dependencies') {
      steps {
        sh 'node -v'
        sh 'npm -v'
        sh 'cd user-service && npm install'
        sh 'cd product-service && npm install'
      }
    }

    stage('Test') {
      steps {
        sh 'cd user-service && npm test'
        sh 'cd product-service && npm test'
      }
    }

    stage('SonarQube Scan') {
      steps {
        withSonarQubeEnv('SonarQube') {
          sh '${SONAR_SCANNER_HOME}/bin/sonar-scanner'
        }
      }
    }

    stage('Build Docker Images') {
      steps {
        sh 'docker build -t user-service ./user-service'
        sh 'docker build -t product-service ./product-service'
      }
    }
  }
}