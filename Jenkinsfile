
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
                        sh 'npm install -g @cyclonedx/cyclonedx-npm'
                    }
                    sh 'npm install'
                }
            }
        }

        stage('Generate SBOM') {
            steps {
                script {
                    if (USE_CYCLONEDX.toBoolean()) {
                        sh "cyclonedx-npm --output-format json --output-file "
                    } else {
                        sh "npm audit --json > "
                    }
                }
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Archive SBOM') {
            steps {
                archiveArtifacts artifacts: "", fingerprint: true
            }
        }
    }
}

