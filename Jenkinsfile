pipeline {
    agent any

    environment {
        IMAGE_NAME = "jenkins-cicd-demo"
        CONTAINER_NAME = "node-web-app"
    }

    stages {
        stage('1. Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/YOUR_GITHUB_USERNAME/jenkins-cicd-demo.git'
            }
        }

        stage('2. Run Tests') {
            steps {
                sh 'docker run --rm -v "$PWD":/app -w /app node:20-alpine npm test'
            }
        }

        stage('3. Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('4. Deploy Application') {
            steps {
                sh '''
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true
                    docker run -d --name $CONTAINER_NAME -p 3000:3000 $IMAGE_NAME:latest
                '''
            }
        }
    }

    post {
        always {
            sh 'docker image prune -f'
        }
    }
}
