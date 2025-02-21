pipeline {
    agent any 

    environment {
        IMAGE_NAME = 'speakeasy:latest' 
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
                    def trivyOutput = sh(script: "trivy image $IMAGE_NAME", returnStdout: true).trim()

                    println trivyOutput

                    if (trivyOutput.contains("Total: 0")) {
                        echo "No vulnerabilities found in the Docker image."
                    } else {
                        echo "Vulnerabilities found in the Docker image."
                    }
                }
            }
        }
    }
}
