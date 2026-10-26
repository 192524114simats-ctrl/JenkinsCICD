```groovy
pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-cicd-app:latest .'
            }
        }

        stage('Test Application') {
            steps {
                sh '''
                    docker run --rm flask-cicd-app:latest \
                    python -m unittest test_app.py
                '''
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                    docker rm -f flask-cicd-container || true

                    docker run -d \
                    --name flask-cicd-container \
                    -p 5000:5000 \
                    flask-cicd-app:latest
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    for i in $(seq 1 15); do
                        if curl -fsS http://docker:5000/health; then
                            exit 0
                        fi
                        sleep 2
                    done

                    echo "Application health check failed"
                    exit 1
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }

        failure {
            echo 'CI/CD Pipeline failed. Check Console Output.'
        }
    }
}
```
