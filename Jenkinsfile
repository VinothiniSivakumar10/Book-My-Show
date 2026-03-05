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
             dir('bookmyshow-app') {
              nodejs('nodejs') {
                sh 'npm install'
               }
             }
           }
        }

        stage('SonarQube Analysis') {
          steps {
            dir('bookmyshow-app') {
              nodejs('nodejs') {
                sh 'npm run sonar'
               }
             }
           }
        }

        stage('Build Docker Image') {
            steps {
              dir('bookmyshow-app'){
                sh 'docker build -t bms-app .'
              }
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 3000:3000 bms-app'
            }
        }
    }
}