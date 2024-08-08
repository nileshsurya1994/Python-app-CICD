pipeline {
    agent any
    
    stages {
        stage("code") {
            steps {
                git url: "https://github.com/nileshsurya1994/Python-app-CICD.git", branch: "main"
                echo 'Code clone successful..'
            }
        }
        
        stage("build and test") {
            steps {
                sh "sudo docker build -t python-app-img ."
                echo 'Build successful'
            }
        }
        
        stage("scan image") {
            steps {
                script {
                    // Pull Trivy image
                    sh "sudo docker pull ghcr.io/aquasec/trivy:latest"
                    
                    // Scan the Docker image with Trivy
                    sh "sudo docker run --rm ghcr.io/aquasec/trivy:latest image --no-progress python-app-img"
                    
                    echo 'Image scanning completed....'
                }
            }
        }
        
        stage("Existing container Remove") {
            steps {
                sh "sudo docker rm -f python-app-run || true"
                echo 'Existing container removed'
            }
        }
        
        stage("deploy") {
            steps {
                sh "sudo docker run -d --name python-app-run -p 80:8000 python-app-img"
                echo 'Deployment completed'
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
