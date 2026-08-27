pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'github-creds', url: 'https://github.com/mugibalankiaq-mugi/backend-project-demo.git'
            }
        }
        stage('Deploy to EC2') {
            steps {
                sshagent(['backend-ssh']) {
                    sh """
                    ssh -o StrictHostKeyChecking=no ubuntu@16.171.151.83 '
                    set -e
                    mkdir -p /home/ubuntu/backend-project-demo
                    cd /home/ubuntu/backend-project-demo
                    rm -rf backend-project-demo
                    git clone https://github.com/mugibalankiaq-mugi/backend-project-demo.git
                    cd backend-project-demo
                    docker stop backend-app || true
                    docker rm backend-app || true
                    docker build -t backend-app .
                    docker run -d --name backend-app -p 5001:5000 backend-app
                    '
                    """
                }
            }
        }
    }
}

