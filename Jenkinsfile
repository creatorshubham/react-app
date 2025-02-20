pipeline {
    agent any

    environment {
        IMAGE_NAME = 'speakeasy:latest' 
    }

    stages {
        

        

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

        
        }
    }
}

pipeline {
    agent any 

    environment {
        DOCKERHUB_CREDENTIALS = credentials('your-docker-credentials')
        APP_NAME = "laly9999/lil-node-app"
    }

    stages { 
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }
        
       stage('Scan Docker Image') {
            steps {
                script {
                    // Run Trivy to scan the Docker image
                    def trivyOutput = sh(script: "trivy image $APP_NAME:latest", returnStdout: true).trim()

                    // Display Trivy scan results
                    println trivyOutput

                    // Check if vulnerabilities were found
                    if (trivyOutput.contains("Total: 0")) {
                        echo "No vulnerabilities found in the Docker image."
                    } else {
                        echo "Vulnerabilities found in the Docker image."
                        // You can take further actions here based on your requirements
                        // For example, failing the build if vulnerabilities are found
                        // error "Vulnerabilities found in the Docker image."
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
