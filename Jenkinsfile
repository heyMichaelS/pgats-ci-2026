pipeline {
  agent {
    docker {
      image 'mcr.microsoft.com/playwright:v1.47.2-jammy'
      args '-u root'
    }
  }

  environment {
    CI = 'true'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Instalar dependencias') {
      steps {
        sh 'corepack enable'
        sh 'yarn install --frozen-lockfile'
      }
    }

    stage('Lint') {
      steps {
        sh 'yarn run lint'
      }
    }

    stage('Testes unitarios') {
      steps {
        sh 'yarn run test'
      }
    }

    stage('Testes E2E') {
      steps {
        sh 'yarn run e2e'
      }
    }

    stage('Testes de mutacao') {
      steps {
        sh 'yarn run test:mutation'
      }
    }
  }

  post {
    always {
      junit allowEmptyResults: true, testResults: 'results.xml'
      archiveArtifacts allowEmptyArchive: true, artifacts: 'reports/**, playwright-report/**, test-results/**, results.xml'
    }
  }
}
