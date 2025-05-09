pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'laravel-app'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/BangEjak04/jenkins-laravel.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Run Laravel Commands') {
            steps {
                sh 'docker-compose exec -T app php artisan config:clear'
                sh 'docker-compose exec -T app php artisan migrate --force'
                sh 'docker-compose exec -T app php artisan test'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8000:8000 $DOCKER_IMAGE'
            }
        }
    }
}
