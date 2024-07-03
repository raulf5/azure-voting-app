/**
 * Jenkins pipeline script for building and testing a Dockerized application.
 */
pipeline {
    agent any
    stages {
        stage('Verify Branch') {
            steps {
                echo "$GIT_BRANCH :D"
            }
        }
        stage('Docker Build') {
            steps {
                // Build Docker images using Docker Compose
                sh(script: 'docker compose build')
            }
        }
        stage('Start App') {
            steps {
                // Start the application containers using Docker Compose
                sh(script: 'docker compose up -d')
            }
        }

        stage('Run Tests') {
            steps {
                // Run tests using pytest
                sh(script: 'pytest ./tests/test_sample.py')
            }
            post {
                success {
                    // Print a success message if tests pass
                    echo 'Tests Passed! :)'
                }
                failure {
                    // Print a failure message if tests fail
                    echo 'Tests Failed! :('
                }
            }
        }

        stage('Docker Push') {
         steps {
            echo "Runnning in $WORKSPACE"
            dir("$WORKSPACE/azure-vote") {
               script {
                  docker.withRegistry('', 'dockerhub') {
                    def image = docker.build("cesarc95/jenkins-masterclass:${new Date().format('yyyyMMdd-HHmmss')}")
                     image.push()
                  }
               }
            }
         }
      }
    }
    /**
     * This Jenkinsfile defines the pipeline for the Azure Voting App with Redis.
     * It includes a post-build step that stops and removes the application containers using Docker Compose.
     */

    post {
        always {
            // Stop and remove the application containers using Docker Compose
            sh(script: 'docker compose down')
        }
    }
}
