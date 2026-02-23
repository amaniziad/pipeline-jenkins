pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Init') {
            steps {
                bat 'mvnw.cmd clean'
            }
        }

        stage('Test') {
            steps {
                bat 'mvnw.cmd test'
                junit '**/target/surefire-reports/*.xml'
            }
        }

        stage('Build') {
            steps {
                bat 'mvnw.cmd install'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                bat 'docker-compose up --build -d'
            }
        }
    }
}