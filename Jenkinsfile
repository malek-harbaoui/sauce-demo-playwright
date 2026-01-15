pipeline {
    agent any

    tools {
        nodejs 'NodeJS-20'
    }

    environment {
        CI = 'true'
    }

    stages {
        stage('📥 Checkout') {
            steps {
                echo '📥 Récupération du code...'
                checkout scm
            }
        }

        stage('📦 Install Dependencies') {
            steps {
                echo '📦 Installation des dépendances npm...'
                bat 'npm ci'
            }
        }

        stage('🌐 Install Browsers') {
            steps {
                echo '🌐 Installation des navigateurs Playwright...'
                bat 'npx playwright install chromium firefox webkit'
            }
        }

        stage('🧪 Test E2E - Chromium') {
            steps {
                echo '🧪 Exécution des tests sur Chromium...'
                bat 'npm run test:e2e -- --project=chromium'
            }
        }

        stage('🧪 Test E2E - Firefox') {
            steps {
                echo '🧪 Exécution des tests sur Firefox...'
                bat 'npm run test:e2e -- --project=firefox'
            }
        }

        stage('🧪 Test E2E - WebKit') {
            steps {
                echo '🧪 Exécution des tests sur WebKit...'
                bat 'npm run test:e2e -- --project=webkit'
            }
        }
    }

    post {
        always {
            echo '📊 Publication des rapports et artifacts...'

            publishHTML([
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'playwright-report',
                reportFiles: 'index.html',
                reportName: 'Playwright Report',
                reportTitles: 'Playwright E2E Test Report'
            ])

            archiveArtifacts artifacts: 'playwright-report/**/*', allowEmptyArchive: true
            archiveArtifacts artifacts: 'screenshots/**/*.png', allowEmptyArchive: true
            archiveArtifacts artifacts: 'test-results/**/*', allowEmptyArchive: true
        }

        success {
            echo '✅ ✅ ✅ TOUS LES TESTS SONT PASSÉS ! ✅ ✅ ✅'
        }

        failure {
            echo '❌ ❌ ❌ DES TESTS ONT ÉCHOUÉ ! ❌ ❌ ❌'
        }
    }
}
