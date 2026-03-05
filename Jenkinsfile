pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'feature-branch', url: 'https://github.com/VinothiniSivakumar10/Book-My-Show.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('bookmyshow-app') {
                    sh 'npm install'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                dir('bookmyshow-app') {
                    withSonarQubeEnv('sonarqube') {
                        sh """
                        ${tool 'sonar-scanner'}/bin/sonar-scanner \
                        -Dsonar.projectKey=bookmyshow \
                        -Dsonar.sources=.
                        """
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('bookmyshow-app') {
                    sh 'docker build -t bookmyshow-app .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 3000:3000 --name bookmyshow-container bookmyshow-app'
            }
        }

        stage('Optional Verification') {
            steps {
                echo "Pipeline executed successfully and container is deployed."
            }
        }

    }

    post {

        success {
            echo 'Pipeline completed successfully!'
            emailext(
                subject: "Jenkins Build SUCCESS",
                body: """
                Good News!

                The Jenkins pipeline for BookMyShow project executed successfully.

                Build Status: SUCCESS
                Project: BookMyShow
                Jenkins Job: ${env.JOB_NAME}
                Build Number: ${env.BUILD_NUMBER}
                """,
                to: "your-email@gmail.com"
            )
        }

        failure {
            echo 'Pipeline failed!'
            emailext(
                subject: "Jenkins Build FAILED",
                body: """
                Attention!

                The Jenkins pipeline has FAILED.

                Project: BookMyShow
                Jenkins Job: ${env.JOB_NAME}
                Build Number: ${env.BUILD_NUMBER}

                Please check Jenkins console logs.
                """,
                to: "your-email@gmail.com"
            )
        }

        always {
            echo "Pipeline execution finished."
        }
    }
}