
pipeline {
    agent any

    environment {
        // Define any environment variables here
      
        //DOCKERHUB_USERNAME = 'your-dockerhub-username'
        //DOCKERHUB_PASSWORD = 'your-dockerhub-password'
        //IMAGE_NAME = 'your-image-name'
        //CONTAINER_NAME = 'your-container-name'
        
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the Git repository
                git 'https://github.com/nileshsurya1994/python-for-devops.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t python-hello-word .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container
                    sh 'docker run -d -p 80:80 python-hello-word'
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker containers and images
            sh 'docker stop $(docker ps -q)'
            sh 'docker rm $(docker ps -a -q)'
            sh 'docker rmi $(docker images -q)'
        }
    }
}
