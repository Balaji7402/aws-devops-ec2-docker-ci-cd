pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "myapp:latest"
        SSH_USER = "ubuntu"
        SSH_HOST = "65.0.89.122"
        SSH_KEY = "/var/lib/jenkins/.ssh/aws.pem"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Balaji7402/aws-devops-ec2-docker-ci-cd.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp .'
            }
        }

        stage('Deploy on EC2') {
            steps {
                sh """
                ssh -i ${SSH_KEY} ${SSH_USER}@${SSH_HOST} '
                    docker stop myapp || true
                    docker rm myapp || true
                    docker run -d -p 80:3000 --name myapp myapp:latest
                '
                """
            }
        }
    }

    post {
        success { echo 'Deployment Successful ✅' }
        failure { echo 'Deployment Failed ❌' }
    }
}
