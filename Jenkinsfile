pipeline {
    agent any

    tools {
        nodejs 'nodejs'
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout Code') {
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
                        script {
                          sh """
                          ${tool 'sonar-scanner'}/bin/sonar-scanner \
                          -Dsonar.projectKey=bookmyshow \
                          -Dsonar.sources=.
                          """
                        }
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
                sh 'docker run -d -p 3000:3000 bookmyshow-app'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yml'
                sh 'kubectl apply -f service.yml'
            }
        }

    }

    post {

        success {
            emailext(
                subject: "Jenkins Build SUCCESS",
                body: """
Pipeline executed successfully.

Project: BookMyShow
Build Number: ${env.BUILD_NUMBER}
Job: ${env.JOB_NAME}

Application deployed successfully.
""",
                to: "vinothinisivakumar31@gmail.com"
            )
        }

        failure {
            emailext(
                subject: "Jenkins Build FAILED",
                body: """
Pipeline execution FAILED.

Project: BookMyShow
Build Number: ${env.BUILD_NUMBER}
Job: ${env.JOB_NAME}

Check Jenkins console logs.
""",
                to: "vinothinisivakumar31@gmail.com"
            )
        }

        always {
            echo "Pipeline finished."
        }
    }
}