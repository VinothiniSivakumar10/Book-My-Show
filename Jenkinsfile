pipeline {
    agent any

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'feature-branch', url:'https://github.com/VinothiniSivakumar10/Book-My-Show.git'
            }
        }

        stage('Install Dependencies') {
            steps {
              nodejs('nodejs') {
              sh 'npm install'
              }
           }
       }

        stage('SonarQube Analysis') {
            steps {
                sh 'npm run sonar'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t bms-app .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 3000:3000 bms-app'
            }
        }
    }
}