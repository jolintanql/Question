pipeline {
    agent any

    environment {
        FLASK_APP = "app.py"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from the repository
                checkout scm
            }
        }
        stage('Build Docker Images') {
            steps {
                script {
                    // Build the Docker images
                    sh 'docker-compose build'
                }
            }
        }
        stage('Start Services') {
            steps {
                script {
                    // Start the Docker services
                    sh 'docker-compose up -d'
                }
            }
        }
        stage('Dependency Check') {
            steps {
                script {
                    // Run dependency check using safety
                    sh 'docker-compose exec flaskapp pip install safety'
                    sh 'docker-compose exec flaskapp safety check'
                }
            }
        }
        stage('Integration Tests') {
            steps {
                script {
                    // Run integration tests
                    sh 'docker-compose exec flaskapp pytest tests/integration'
                }
            }
        }
        stage('UI Tests') {
            steps {
                script {
                    // Run UI tests using Selenium
                    sh 'docker-compose exec flaskapp pytest tests/ui'
                }
            }
        }
    }

    post {
        always {
            // Clean up the Docker containers
            script {
                sh 'docker-compose down'
            }
        }
        success {
            // Archive the test results
            archiveArtifacts artifacts: 'tests/reports/**/*.xml', allowEmptyArchive: true
        }
        
    }
}
