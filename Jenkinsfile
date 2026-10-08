```groovy
pipeline {
    agent any

    stages {

        stage('Run Selenium Tests with pytest') {
            steps {
                echo "Running Selenium Tests using pytest"

                // Check Python installation
                bat 'python --version'
                bat 'python -m pip --version'

                // Install Python dependencies
                bat 'python -m pip install -r requirements.txt'

                // Start Flask app in background
                bat 'start /B python app.py'

                // Wait for Flask server to start
                bat 'ping 127.0.0.1 -n 5 > nul'

                // Run Selenium tests
                bat 'python -m pytest -v'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker Image"

                bat 'docker build -t registration-form:v1 .'
            }
        }

        stage('Docker Login') {
            steps {
                echo "Logging into Docker Hub"

                bat 'docker login -u saibhavani12 -p docker@sai12'
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                echo "Pushing Docker Image to Docker Hub"

                bat 'docker tag registration-form:v1 saibhavani12/sample2026:registration-form'

                bat 'docker push saibhavani12/sample2026:registration-form'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo "Deploying application to Kubernetes"

                bat 'kubectl apply -f deployment.yaml --validate=false'
                bat 'kubectl apply -f service.yaml'

                echo "Checking Kubernetes resources"

                bat 'kubectl get deployments'
                bat 'kubectl get pods'
                bat 'kubectl get services'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
```
