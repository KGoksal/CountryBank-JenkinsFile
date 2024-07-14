pipeline {
    agent any  // Executes on any available agent

    stages {
        stage('Git Checkout') {
            steps {
                // Step to checkout code from Git repository
                git 'https://github.com/KGoksal/CountryBank-JenkinsFile.git'
            }
        }

        stage('OWASP Dependency Check') {
            steps {
                script {
                    // Run OWASP Dependency Check
                    dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DC'
                    // Publish Dependency Check results as artifacts
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
                }
            }
        }

        stage('Trivy') {
            steps {
                script {
                    // Run Trivy with quiet mode enabled
                    sh "trivy fs ."
                }
            }
        }

        stage('Build & deploy') {
            steps {
                script {
                    // Example command to build and deploy using Docker Compose
                    sh "docker-compose up -d"
                }
            }
        }
    }
}
