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

        stage('Clean up existing containers') {
            steps {
                script {
                    // Stop and remove all running containers
                    sh 'docker ps -aq && docker stop $(docker ps -aq) || true && docker rm $(docker ps -aq) || true'
                }
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
                sh 'docker compose exec -T app cp .env.example .env'
                sh 'docker compose exec -T app php artisan key:generate'
                sh 'docker compose exec -T app php artisan config:clear'
                sh 'docker compose exec -T app php artisan migrate:fresh --seed'
                sh 'docker compose exec -T app php artisan test'
            }
        }
    }
}
