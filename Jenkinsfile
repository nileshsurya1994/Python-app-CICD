pipeline {
    agent any
    
    stages {
        stage("Code") {
            steps {
                git url: "https://github.com/nileshsurya1994/Python-app-CICD.git", branch: "main"
                echo 'Code clone successful...'
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
        
        stage("Scan Image") {
            steps {
                script {
                    // Authenticate Docker with GitHub Container Registry
                    sh """
                        echo "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" | sudo docker login ghcr.io -u nileshsurya1994 --password-stdin
                    """
                    
                    // Pull Trivy image
                    sh "sudo docker pull ghcr.io/aquasec/trivy:latest"
                    
                    // Scan the Docker image with Trivy
                    sh "sudo docker run --rm ghcr.io/aquasec/trivy:latest image --no-progress python-app-img"
                    
                    echo 'Image scanning completed...'
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
