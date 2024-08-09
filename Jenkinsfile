pipeline {
    agent agent

    stages {
        stage("Code") {
            steps {
                git url: "https://github.com/nileshsurya1994/Python-app-CICD.git", branch: "main"
                echo 'Code clone successful.....'
            }
        }
        
        stage("Build and Test") {
            steps {
                sh "sudo docker build -t python-app-img ."
                echo 'Build successful'
            }
        }
        
        stage("Remove Existing Container") {
            steps {
                sh "sudo docker rm -f python-app-run || true"
                echo 'Existing container removed'
            }
        }
        
        stage("Deploy") {
            steps {
                sh "sudo docker run -d --name python-app-run -p 80:8000 python-app-img"
                echo 'Deployment completed'
            }
        }
        
        stage("Scan Code and Image") {
            steps {
                script {
                    withCredentials([string(credentialsId: 'github-pat', variable: 'GITHUB_PAT')]) {
                        // Authenticate Docker with GitHub Container Registry
                        sh """
                            echo ${GITHUB_PAT} | sudo docker login ghcr.io -u nileshsurya1994 --password-stdin
                        """
                        
                        // Ensure Docker is running and accessible
                        sh "sudo docker info || echo 'Docker info failed'"

                        // Pull Trivy image from Docker Hub
                        sh "sudo docker pull aquasec/trivy:latest || echo 'Failed to pull Trivy image'"
                        
                        // Scan the codebase with Trivy
                        sh 'sudo docker run --rm -v $(pwd):/src aquasec/trivy:latest fs /src || echo "Failed to scan code"'

                        // Scan the Docker image with Trivy
                        sh "sudo docker run --rm aquasec/trivy:latest image --no-progress python-app-img || echo 'Failed to scan image'"

                        echo 'Code and Image scanning completed!!'
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline successfully executed!'
        }
        failure {
            echo 'Pipeline failed :('
        }
    }
}
