pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'sayeda77/ruby-postgresql_app:latest' // Change this to your Docker Hub username and desired image name
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Building the Docker image
                    sh "docker build -t $DOCKER_IMAGE ."

                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run tests if applicable
                    // e.g., RSpec or other test suites for Rails
                    sh "docker run --rm $DOCKER_IMAGE bundle exec rspec" // Example RSpec command
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    // Logging into Docker Hub
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                       sh "echo \$DOCKERHUB_PASSWORD | docker login --username \$DOCKERHUB_USERNAME --password-stdin"

                    }

                    // Pushing the image to Docker Hub
                    sh "docker push $DOCKER_IMAGE"

                }
            }
        }
    }

    post {
        success {
            echo "Image successfully pushed to Docker Hub!"
        }

        failure {
            echo "Failed to push the image to Docker Hub!"
        }
    }
}

