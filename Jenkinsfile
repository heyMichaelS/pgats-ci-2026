pipeline {
  agent any

  tools {
    nodejs 'Node 24'
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
        sh 'npm install -g yarn'
        sh 'yarn install --frozen-lockfile'
        sh 'npx playwright install --with-deps chromium'
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
