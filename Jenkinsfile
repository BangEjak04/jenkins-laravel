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
                    sh 'sudo docker ps -aq && docker stop $(docker ps -aq) || true && docker rm $(docker ps -aq) || true'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'sudo docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Run Laravel Commands') {
            steps {
                sh 'sudo docker compose up -d'
                sh 'sudo docker compose exec -T app composer install --no-interaction --prefer-dist'
                sh 'sudo docker compose exec -T app cp .env.example .env'
                sh 'sudo docker compose exec -T app php artisan key:generate'
                sh 'sudo docker compose exec -T app php artisan config:clear'
                sh 'sudo docker compose exec -T app php artisan migrate:fresh --seed'
                sh 'sudo docker compose exec -T app php artisan test'
            }
        }
    }
}
