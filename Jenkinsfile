
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Clone the Git repository
                git 'https://github.com/nileshsurya1994/Python-app-CICD.git'
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
