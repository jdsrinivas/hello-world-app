pipeline {
    agent any

    environment {
        SBOM_FILE = "bom.json"
        USE_CYCLONEDX = true
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    echo 'Simulating SCM Checkout'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    if (USE_CYCLONEDX.toBoolean()) {
                        bat 'npm install -g @cyclonedx/cyclonedx-npm'
                    }
                    bat 'npm install'
                }
            }
        }

        stage('Generate SBOM') {
            steps {
                script {
                    if (USE_CYCLONEDX.toBoolean()) {
                        bat "cyclonedx-npm --output-format json --output-file ${SBOM_FILE}"
                    } else {
                        bat "npm audit --json > ${SBOM_FILE}"
                    }
                }
            }
        }

        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }

        stage('Archive SBOM') {
            steps {
                archiveArtifacts artifacts: "${SBOM_FILE}", fingerprint: true
            }
        }
    }
}