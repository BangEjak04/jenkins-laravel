pipeline {
    agent any

    environment {
        COMPOSER_ALLOW_SUPERUSER = 1
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/BangEjak04/jenkins-laravel.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker-compose build'
            }
        }

        stage('Start Services') {
            steps {
                sh 'docker-compose up -d'
            }
        }

        stage('Run Laravel Commands') {
            steps {
                sh 'docker-compose exec -T app php artisan config:clear'
                sh 'docker-compose exec -T app php artisan migrate --force'
                sh 'docker-compose exec -T app php artisan test'
            }
        }

        stage('Cleanup') {
            steps {
                sh 'docker-compose down'
            }
        }
    }
}
