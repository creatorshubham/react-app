pipeline {
    agent any

    environment {
        IMAGE_NAME = 'speakeasy:latest' 
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub or your repository
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                script {
                    // Build the Docker image (if necessary)
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Run Trivy Scan') {
            steps {
                script {
                    // Run Trivy scan with severity filtering directly
                    def trivyExitCode = sh(script: "trivy image ${IMAGE_NAME} --severity CRITICAL --quiet", returnStatus: true)

                    // If the exit code is non-zero, it means vulnerabilities were found
                    if (trivyExitCode != 0) {
                        echo "Critical vulnerabilities detected during Trivy scan. Failing the build."
                        currentBuild.result = 'FAILURE'
                        return
                    } else {
                        echo "No critical vulnerabilities detected."
                    }
                }
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    echo "Assume your application has been deployed."
                }
            }
        }
    }
}
