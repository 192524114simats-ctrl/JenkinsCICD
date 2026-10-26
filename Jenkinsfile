pipeline {
    agent any

    stages {
        stage('Check Environment') {
            steps {
                bat 'docker --version'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t flask-cicd-app:latest .'
            }
        }

        stage('Test Application') {
            steps {
                bat 'docker run --rm flask-cicd-app:latest python -m unittest test_app.py'
            }
        }

        stage('Deploy Application') {
            steps {
                bat 'docker rm -f flask-cicd-container'
                bat 'docker run -d --name flask-cicd-container -p 5000:5000 flask-cicd-app:latest'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
