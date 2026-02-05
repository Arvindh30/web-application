pipeline {
    agent { label 'DSlave' }

    stages {

        stage('Cloning') {
            steps {
                git branch: 'master', url: 'https://github.com/Arvindh30/web-application.git'
            }
        }

        stage('Cleanup Docker Images') {
            steps {
                sh '''
                    echo "Stopping running containers..."
                    sudo docker-compose down || true
                    sudo docker stop $(sudo docker ps -q) || true
                    sudo docker rm $(sudo docker ps -aq) || true

                    echo "Removing old images..."
                    sudo docker rmi -f $(sudo docker images -q) || true
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    sudo docker build --no-cache -t worimg:latest .
                '''
            }
        }

        stage('Docker Compose Up') {
            steps {
                sh '''
                    sudo docker-compose up -d
                '''
            }
        }
    }
}
