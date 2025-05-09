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
                sh 'docker compose up -d'
                sh 'docker compose exec -T app composer install --no-interaction --prefer-dist'
                sh 'docker compose exec -T app php artisan config:clear'
                sh 'docker compose exec -T app php artisan key:generate'
                sh 'docker compose exec -T app php artisan migrate --seed'
                sh 'docker compose exec -T app php artisan test'
            }
        }


        stage('Run Container') {
            steps {
                sh 'docker run -d -p 8000:8000 $DOCKER_IMAGE'
            }
        }
    }
}
